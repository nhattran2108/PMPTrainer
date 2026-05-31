# CLAUDE.md — AI Dev Guide for PMP Trainer Cloud

## Dev Rules
- Fix code directly, no explanations unless asked
- Edit only files relevant to the task
- No comments in code unless logic is non-obvious

## File Structure

```
public/
  index.html   # HTML + CSS only (no JS logic)
  app.js       # ALL JS: questions array, app logic, auth, sync, sidebar
sql/
  schema.sql   # Supabase schema (run once in SQL Editor)
```

## Architecture

Cloud-synced PMP Trainer. Supabase auth + DB layered on top of localStorage app.

### Database (Supabase)
- `profiles` — user info (auto-created via trigger on signup)
- `progress` — answered/correct/wrong/currentIdx/mode/flagged (1 row per user)
- `imported_questions` — user-uploaded question sets (1 row per user)
- `exam_history` — mock exam results (many rows per user)
- RLS: users only access their own data

### Sync
- `saveToCloud()` — upserts progress + imported questions
- `loadFromCloud()` — reads and merges with localStorage
- `debouncedSync()` — called on every `saveSession()`, 2s debounce
- Conflict resolution: more answered questions wins

### Auth Flow
1. "Sign In" → modal opens
2. Email/password or Google OAuth
3. On success → `onAuthSuccess()` → `loadFromCloud()`
4. `supaClient.auth.onAuthStateChange()` handles Google redirect

## Critical Notes

### Supabase Credentials
Anon key is public/safe in frontend. Security via RLS, not key secrecy.
```js
var SUPABASE_URL = 'https://xxx.supabase.co';
var SUPABASE_KEY = 'eyJ...';
```

### Monkey-patching
Auth/sync patches existing functions without modifying originals:
```js
var _origSaveSession = saveSession;
saveSession = function(){ _origSaveSession(); debouncedSync(); };
```

### Offline Fallback
If `SUPABASE_URL === 'YOUR_SUPABASE_URL'`, `supaClient` stays null, all sync silently skips.

## Sidebar Architecture

### Mobile (≤768px) — Overlay mode
- Sidebar: `transform:translateX(-100%)` + `visibility:hidden` (off-screen)
- Open: add `.open` class → `translateX(0)` + `visibility:visible`
- Dark overlay (`#sidebar-overlay`) covers content when open
- Close methods: X button inside sidebar, tap overlay, swipe left
- Hamburger wired via `touchend` (avoids iOS 300ms click delay) + `click` fallback

### Desktop (>768px) — Push mode
- Sidebar always at `left:0`, `#app` has `margin-left:248px`
- Toggle adds `body.nav-closed` → sidebar `translateX(-100%)`, app `margin-left:0`
- State persisted in `localStorage('nav-closed')`

### Key functions in app.js
```js
isMobile()         // returns window.innerWidth <= 768
toggleSidebar()    // routes to mobile overlay or desktop push
closeSidebar()     // closes without toggle
```

## iOS Scroll Fix
All flex containers in the scroll chain need `min-height:0`:
```
body (overflow:hidden)
 └─ #app (min-height:0, overflow:hidden)
     └─ #main (min-height:0, overflow:hidden)
         ├─ #topbar (flex-shrink:0)
         └─ #content (min-height:0, overflow-y:auto) ← scroll here
```

## Pending Setup
- SQL migration: `ALTER TABLE progress ADD COLUMN IF NOT EXISTS flagged jsonb DEFAULT '{}';`
- Google OAuth: enable in Supabase Dashboard → Auth → Providers + Google Cloud Console
- Supabase Auth → URL Configuration → set Site URL to Vercel domain

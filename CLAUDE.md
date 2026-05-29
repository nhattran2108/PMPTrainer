# CLAUDE.md — AI Dev Guide for PMP Trainer Cloud

## Architecture

This is the cloud-synced version of PMP Trainer. It adds Supabase auth + database on top of the original single-file app.

### Frontend (public/index.html)
- Same single-file architecture as the original
- Supabase JS client loaded from CDN
- Auth modal (email/password + Google OAuth)
- Auto-sync layer patches existing `saveSession()` and `confirmImport()`

### Database (Supabase)
- `profiles` — user info, auto-created on signup via trigger
- `progress` — answered/correct/wrong/currentIdx/mode (1 row per user)
- `imported_questions` — user-uploaded question sets (1 row per user)
- `exam_history` — mock exam results (many rows per user)
- All tables have RLS policies: users only access their own data

### Sync Strategy
- `saveToCloud()` — upserts progress + imported questions
- `loadFromCloud()` — reads and merges with localStorage
- `debouncedSync()` — called on every `saveSession()`, waits 2s before actual sync
- Conflict resolution: whichever has more answered questions wins

### Auth Flow
1. User clicks "Sign In" → modal opens
2. Email/password or Google OAuth
3. On success → `onAuthSuccess()` → `loadFromCloud()`
4. `supaClient.auth.onAuthStateChange()` handles Google redirect callback

## Critical Notes

### Supabase Credentials
The anon key is PUBLIC and safe for frontend. It's embedded in index.html:
```js
var SUPABASE_URL = 'https://xxx.supabase.co';
var SUPABASE_KEY = 'eyJ...';
```
Security comes from RLS policies, not key secrecy.

### Monkey-patching
Auth/sync code patches existing functions without modifying them:
```js
var _origSaveSession = saveSession;
saveSession = function(){ _origSaveSession(); debouncedSync(); };
```
This keeps the original app code untouched and makes the auth layer removable.

### Offline Fallback
If `SUPABASE_URL` equals `'YOUR_SUPABASE_URL'` (not configured), `supaClient` stays null and all sync functions silently skip. The app works exactly like the original localStorage-only version.

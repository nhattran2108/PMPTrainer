# PMP Trainer Cloud

PMP Exam Trainer with **cloud sync** via Supabase. Progress syncs across all your devices.

## Quick Start

### Step 1: Create Supabase Project

1. [supabase.com](https://supabase.com) → **Start your project** (free)
2. Create new project → wait ~1 min

### Step 2: Run Database Schema

1. Supabase Dashboard → **SQL Editor** → **New query**
2. Paste contents of `public/schema.sql` → **Run**

### Step 3: Add Flagged Questions Column

Run this migration in SQL Editor:
```sql
ALTER TABLE progress ADD COLUMN IF NOT EXISTS flagged jsonb DEFAULT '{}';
```

### Step 4: Configure Credentials

Edit `public/index.html`, find these 2 lines near the top:
```js
var SUPABASE_URL  = 'YOUR_SUPABASE_URL';
var SUPABASE_KEY  = 'YOUR_SUPABASE_ANON_KEY';
```
Replace with values from Supabase Dashboard → **Settings** → **API**.

### Step 5: Deploy to Vercel

```bash
git init
git remote add origin https://github.com/YOUR_USER/PMPTrainerCloud.git
git add .
git commit -m "init"
git push -u origin main
```
Vercel → New Project → select repo → Deploy.

Then: Supabase Dashboard → **Authentication** → **URL Configuration** → set **Site URL** to your Vercel URL.

### Step 6: Enable Google Login (optional)

Supabase Dashboard → **Authentication** → **Providers** → Enable **Google** → add OAuth credentials from Google Cloud Console.

---

## Features

- 1,387 PMP practice questions
- Practice mode & mock exam mode
- Flag questions for review
- Progress tracked per question (correct/wrong/skipped)
- Cloud sync across devices (Supabase)
- Offline mode (localStorage fallback)
- Responsive: desktop push sidebar + mobile overlay sidebar
- iOS scroll support for long questions

## How Sync Works

- **Logged in**: Every answer auto-syncs (debounced 2s)
- **Not logged in**: localStorage only (same as before)
- **New device login**: Cloud progress loads and merges
- **Conflict**: More answered questions wins

## What Syncs

| Data | Synced |
|------|--------|
| Practice progress (answered/correct/wrong) | ✅ |
| Flagged questions | ✅ |
| Imported questions | ✅ |
| Mock exam history | ✅ |
| Current question index & mode | ✅ |

## Project Structure

```
PMPTrainerCloud/
├── public/
│   ├── index.html      # HTML + CSS
│   ├── app.js          # All JS logic (questions, app, auth, sync)
│   └── schema.sql      # Supabase database schema
├── vercel.json
├── package.json
├── CLAUDE.md           # AI dev guide
└── README.md
```

## Security

- Supabase Anon Key is public (safe in frontend)
- Row Level Security (RLS): users only access their own data
- Passwords hashed by Supabase Auth (bcrypt)

## Cost

Both free tiers:
- **Supabase**: 500MB DB, 50K auth users
- **Vercel**: 100GB bandwidth, HTTPS, custom domain

## License

MIT

# PMP Trainer Cloud

PMP Exam Trainer with **cloud sync** via Supabase. Progress syncs across all your devices.

## Quick Start (4 steps)

### Step 1: Create Supabase Project (2 min)

1. Go to [supabase.com](https://supabase.com) → **Start your project** (free)
2. Create a new project, pick a name and password
3. Wait ~1 min for it to set up

### Step 2: Run Database Schema (1 min)

1. In Supabase Dashboard → **SQL Editor** → **New query**
2. Copy-paste the contents of `sql/schema.sql`
3. Click **Run** → all tables and policies are created

### Step 3: Enable Google Login (optional, 2 min)

1. Supabase Dashboard → **Authentication** → **Providers**
2. Enable **Google**
3. Follow the instructions to set up Google OAuth credentials
4. Or skip this — email/password login works without it

### Step 4: Configure & Deploy (2 min)

1. Get your credentials from Supabase Dashboard → **Settings** → **API**:
   - `Project URL` (e.g., `https://abc123.supabase.co`)
   - `anon public` key (starts with `eyJ...`)

2. Edit `public/index.html`, find these 2 lines near the top:
   ```js
   var SUPABASE_URL  = 'YOUR_SUPABASE_URL';
   var SUPABASE_KEY  = 'YOUR_SUPABASE_ANON_KEY';
   ```
   Replace with your actual values:
   ```js
   var SUPABASE_URL  = 'https://abc123.supabase.co';
   var SUPABASE_KEY  = 'eyJhbGciOiJIUzI1NiIs...';
   ```

3. Deploy to Vercel:
   ```bash
   # Push to GitHub
   git init
   git remote add origin https://github.com/YOUR_USER/PMPTrainerCloud.git
   git add .
   git commit -m "PMP Trainer with cloud sync"
   git push -u origin main

   # Then go to vercel.com → New Project → select your repo → Deploy
   ```

4. Done! Visit your Vercel URL → Sign in → Progress syncs everywhere.

## How Sync Works

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Device A    │     │   Supabase   │     │  Device B    │
│  (laptop)    │────▶│   Database   │◀────│  (phone)     │
│              │     │              │     │              │
│ localStorage │     │  progress    │     │ localStorage │
│ + cloud sync │     │  imports     │     │ + cloud sync │
│              │     │  exam_history│     │              │
└──────────────┘     └──────────────┘     └──────────────┘
```

- **Logged in**: Every answer auto-syncs to Supabase (debounced 2s)
- **Not logged in**: Works offline with localStorage (same as before)
- **Login on new device**: Cloud progress auto-loads and merges
- **Conflict**: Whichever has more answered questions wins

## What Syncs

| Data | Synced | Storage |
|------|--------|---------|
| Practice progress (answered/correct/wrong) | ✅ | Supabase `progress` table |
| Imported questions | ✅ | Supabase `imported_questions` table |
| Mock exam history | ✅ | Supabase `exam_history` table |
| Current question index & mode | ✅ | Supabase `progress` table |
| Drag & drop state | ❌ | In-memory only (resets on reload) |

## Project Structure

```
PMPTrainerCloud/
├── public/
│   └── index.html          # Complete app (HTML + CSS + JS + data)
├── sql/
│   └── schema.sql          # Supabase database schema
├── vercel.json             # Vercel deployment config
├── package.json            # Project metadata
├── .env.example            # Environment variables template
├── CLAUDE.md               # AI development guide
└── README.md               # This file
```

## Security

- **Supabase Anon Key** is public (safe to put in frontend code)
- **Row Level Security (RLS)** ensures users can only read/write their own data
- **Passwords** are hashed by Supabase Auth (bcrypt)
- **No server-side code** needed — Supabase handles everything

## Offline Mode

The app works 100% without Supabase:
- If `SUPABASE_URL` is not set → app uses localStorage only
- If network is down → localStorage cache keeps working
- Click "Skip - use offline" on the login modal

## Cost

Everything is free:
- **Supabase Free Plan**: 500MB database, 50K auth users, unlimited API
- **Vercel Free Plan**: 100GB bandwidth, custom domain, HTTPS
- **No credit card** required for either

## License

MIT

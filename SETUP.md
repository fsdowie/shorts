# ☘️ Ireland Trip Planner — Setup Guide

This page works in **demo mode** out of the box (ideas saved locally in your browser).
To enable **live sharing with friends**, follow the steps below to connect Supabase (free).

---

## Step 1 — Create a free Supabase project

1. Go to [https://supabase.com](https://supabase.com) and sign up / log in.
2. Click **"New project"**, give it a name (e.g. `ireland-trip`) and choose a region close to Ireland or Europe.
3. Wait ~2 minutes for the project to provision.

---

## Step 2 — Create the database table

1. In your Supabase project, click **"SQL Editor"** in the left sidebar.
2. Paste the following SQL and click **"Run"**:

```sql
-- Create the ideas table
create table ireland_ideas (
  id         uuid        default gen_random_uuid() primary key,
  handle     text        not null,
  category   text        not null,
  message    text        not null,
  created_at timestamptz default now()
);

-- Enable Row-Level Security (required by Supabase)
alter table ireland_ideas enable row level security;

-- Allow anyone with the anon key to read and insert
-- (perfect for a friends-only shared page)
create policy "Allow public read"   on ireland_ideas for select using (true);
create policy "Allow public insert" on ireland_ideas for insert with check (true);

-- Enable real-time so friends see new ideas instantly
alter publication supabase_realtime add table ireland_ideas;
```

---

## Step 3 — Add your credentials to the HTML file

1. In Supabase, go to **Settings → API**.
2. Copy:
   - **Project URL** (looks like `https://xxxxxxxxxxxx.supabase.co`)
   - **anon / public key** (the long JWT string)
3. Open `ireland-trip-planner.html` in a text editor and find these two lines near the top of the `<script>` section:

```js
const SUPABASE_URL      = 'YOUR_SUPABASE_URL_HERE';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY_HERE';
```

4. Replace the placeholder strings with your actual values. For example:

```js
const SUPABASE_URL      = 'https://abcdefgh.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5...';
```

---

## Step 4 — Publish to GitHub Pages

1. Create a new **public** GitHub repository (e.g. `ireland-trip`).
2. Push both files (`ireland-trip-planner.html` and `SETUP.md`) to the repo.
3. In the repo settings → **Pages**, set source to `main` branch / `root`.
4. Your page will be live at:
   `https://<your-username>.github.io/ireland-trip/ireland-trip-planner.html`
5. Share that URL with your friends — everyone gets real-time updates! 🎉

---

## Tips

- **Each friend** will be prompted for their name/handle the first time they open the page. It's saved in their browser automatically.
- The **Idea Board** on the right groups ideas by category — great for planning sessions.
- Ideas are stored permanently in Supabase and visible to anyone with the link.
- You can view, export, or delete ideas at any time from the Supabase **Table Editor**.

---

## Categories

| Icon | Category | Use it for |
|------|----------|------------|
| 🎯 | Things To Do | Hikes, tours, sightseeing, activities |
| 📅 | Available Dates | When each person is free to travel |
| 🏠 | Places To Stay | Hotels, Airbnbs, hostels, locations |
| 🍺 | Food & Pubs | Restaurants, traditional pubs, must-eats |
| 💬 | General | Anything else — notes, questions, links |

---

*Sláinte! 🍀*

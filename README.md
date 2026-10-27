# Metro Seller Survey — internal field app

An interactive, tablet-first survey app your team runs in the field. 4–5 people can each
have this one file on their iPad/tablet; every response syncs to **one shared database**.

- Opens with a **credibility showcase** — pick the owner's industry and show the big SingleInterface clients in that space (logos + name), the "Asia's Largest Full-Stack AI Retail Tech Platform" stats, and trust badges, to build trust before the interview
- **Surfaces real need, doesn't pitch** — Section G asks about actual behaviour (enquiry volume, who answers, what happens to missed/after-hours enquiries, whether Google is ever updated, how reviews are handled, "hand off ONE thing") so product fit is *inferred*, not led by yes/no questions
- One question per card, auto-advances on single-select, progress bar + confetti finish
- Each survey starts by recording the **interviewee** (business / shop name + owner name)
- **☰ Jump to section / question** drawer — switch sections or jump to any question (✓ marks answered)
- Multi-select questions with big, stacked, easy-to-read options
- Surveyor (interviewer) signs in with their name once → every response is tagged to who collected it
- **Offline-safe**: if the signal drops, responses are saved on the device and auto-sync when back online
- **Results tab**: live tally across all tablets + headline signals + one-click CSV export

---

## ⭐ How configuration works (read this first)

You add the Supabase keys **once, in one place, on your laptop** — then copy that
same configured file to every tablet. **Nobody configures keys per person.**

```
  YOU (once)                          EACH SURVEYOR (on their tablet)
  ───────────                         ──────────────────────────────
  1. Make Supabase project            • Open the file
  2. Run the SQL (one time)           • Type their name once
  3. Paste 2 keys into index.html     • Start surveying
  4. Copy that file to all 5 tablets    (keys are already inside — they never touch them)
```

The keys live inside `index.html`. Once they're in, they travel with the file. So the
flow is: **configure one master file → duplicate it to each device → each person just
signs in with their name.** That's the whole thing.

---

## One-time setup (≈5 minutes) — do this once, then share the file

### 1. Create a free Supabase project
1. Go to **https://supabase.com** → sign in → **New project** (free tier is fine).
2. Give it a name, set a database password, pick a region near you, create it.

### 2. Create the responses table
In Supabase, open **SQL Editor** → **New query**, paste this, and click **Run**:

```sql
create extension if not exists pgcrypto;

create table public.responses (
  id uuid primary key default gen_random_uuid(),
  created_at timestamptz not null default now(),
  surveyor text,
  device text,
  client_time timestamptz,
  payload jsonb not null default '{}'::jsonb
);

alter table public.responses enable row level security;

-- Internal tool: allow the app's anon key to add and read responses.
create policy "anon insert" on public.responses
  for insert to anon with check (true);
create policy "anon read" on public.responses
  for select to anon using (true);
```

### 3. Get your keys
In Supabase: **Project Settings → API**. Copy:
- **Project URL** (looks like `https://abcd1234.supabase.co`)
- **anon public** key (a long string — the *anon* one, NOT the service_role key)

### 4. Paste them into the app
Open `index.html` in any text editor. Near the top, edit the `CONFIG` block:

```js
const CONFIG = {
  SUPABASE_URL: "https://abcd1234.supabase.co",   // your Project URL
  SUPABASE_ANON_KEY: "eyJhbG...your anon key...",  // your anon public key
  TEAM: ["Nishtha","Priya","Rahul","Amit","Sana"]  // your 4–5 surveyor names
};
```

Save. That's it — the "not set up" warning disappears once keys are in.

---

## Put it on each tablet
1. Email/AirDrop/USB the `index.html` file to each tablet (same file for everyone).
2. Open it in the browser (Safari on iPad, Chrome on Android).
3. **iPad:** Share → **Add to Home Screen** → it opens full-screen like a real app.
4. Each person taps their name once; the app remembers it on that device.

All tablets now write to the **same** database. Open the **Results** tab on any device to see
the combined live tally, or tap **Download CSV** for the full export.

> Tip: keep one "master" copy of `index.html` with your keys already filled in, and copy
> that same file to every tablet.

---

## Editing the survey questions
All questions live in the `SECTIONS` array in `index.html`. Each question looks like:

```js
{ id:"role", q:"What is your role?", opts:["Owner","Partner","Manager"] }           // single-select
{ id:"sources", q:"Where do customers come from?", multi:true, opts:[...] }          // multi-select
{ id:"problems", q:"Biggest problems?", multi:true, max:3, opts:[...] }              // multi, max 3
{ id:"age", q:"Your age", opts:[...], noOther:true }                                 // hides the "Other" option
```

- Give every question a **unique `id`** (used as the database/CSV column).
- `multi:true` = checkboxes (pick many). No `multi` = single pick (auto-advances).
- `noOther:true` removes the "Other → type it" option.
- Change section emoji/titles/blurbs at the top of each section block.

---

## Where the data lives
- **Shared:** Supabase table `public.responses` (one row per survey, `payload` holds the answers).
- **On each device (temporary):** a small offline queue in the browser, cleared automatically once synced.
- **Export:** Results tab → Download CSV (one row per response, one column per question, plus surveyor + contact + notes).

## Troubleshooting
- **"not set up"** → `CONFIG` keys are blank. Add them (step 4).
- **Dot stays red / "offline"** → no internet, or wrong URL/key. Responses are still safe on the device and sync later.
- **Nothing in Results** → check the table + policies (step 2) ran without error, and you're online. Tap **↻ Refresh**.
- **"permission denied" on save** → the RLS policies in step 2 didn't apply; re-run that SQL.

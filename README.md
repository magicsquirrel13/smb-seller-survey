# Metro Seller Survey — internal field app

An interactive, tablet-first survey app your team runs in the field. 4–5 people can each
have this one file on their iPad/tablet; every response syncs to **one shared database**.

- **Every question is compulsory.** Next, the Jump drawer and Save all refuse to move past an
  unanswered question or an empty required field, with a clear message of what is missing
- Each survey starts with the **interviewee card**: business name, city, owner name, phone,
  email, **Google Business Profile link**, a **photo of the place**, plus gender, language and
  business type — all required
- **Surfaces real need, doesn't pitch** — Section G asks about actual behaviour (what matters,
  how well it is handled today, what they have tried) so product fit is *inferred*, not led
- Section D asks whether the owner **already uses AI** for the business and, if so, how
- Section I checks **focus-group availability** for 10–20 December: yes / maybe / no, which
  dates, and a morning / afternoon / evening slot
- One question per card, auto-advances on single-select, progress bar + confetti finish
- **EN / Hinglish** toggle — the surveyor can read every question in Hinglish; stored answers
  stay in English
- **☰ Jump to section / question** drawer — ✓ marks answered questions, 🔒 marks questions you
  cannot reach yet because an earlier one is still unanswered
- Surveyor signs in with their name once → every response is tagged to who collected it
- **Offline-safe**: if the signal drops, responses are saved on the device and auto-sync later
- **Results tab (admin only)**: live tally across all tablets, headline signals, a focus-group
  shortlist with contacts and GBP links, every free-text note, and one-click CSV export

---

## The survey flow

| Step | What happens | Required |
|---|---|---|
| Interviewee card | Business, city, owner, phone (10+ digits), email, GBP link (https://…), photo, gender, language, business type | All of it |
| A · About you | Role, generation, age, education | Every shown question |
| B · Your business | Outlets, age of business, staff, monthly sales | Every shown question |
| C · How you get customers today | Online presence, customer sources | Every shown question |
| D · Your digital marketing | Who runs it, ads, agency, spend, satisfaction, **AI usage + how** | Every shown question |
| E · Pain point | Biggest problem in their own words + closest code, customers lost per week | Every shown question |
| F · What's happening today | Live checks on the phone: Google presence, competitors, enquiries, missed calls | Every shown question |
| G · Need gap | 6 jobs × importance + how well handled (grid), time spent, what they have tried | All 12 grid cells |
| H · Product fit | Which service first, free trial, reason, price acceptance | Every shown question |
| I · Focus group | Available 10–20 Dec?, which dates, time of day | Every shown question |
| Notes | Interviewer's free notes | Optional |
| Finish | Save to the shared database | Blocked until nothing is missing |

Questions that depend on an earlier answer (e.g. agency fee, AI "how", focus-group dates) only
appear when relevant, and only those are required. If "Other" is picked, the typed answer is
required too.

---

## ⭐ How configuration works (read this first)

You add the Supabase keys **once, in one place, on your laptop** — then copy that
same configured file to every tablet. **Nobody configures keys per person.**

```
  YOU (once)                          EACH SURVEYOR (on their tablet)
  ───────────                         ──────────────────────────────
  1. Make Supabase project            • Open the file
  2. Run the SQL (one time)           • Type their name once
  3. Create the photo bucket          • Start surveying
  4. Create one admin login             (keys are already inside — they never touch them)
  5. Paste 2 keys into index.html
  6. Copy that file to all tablets
```

The keys live inside `index.html`. Once they're in, they travel with the file. So the
flow is: **configure one master file → duplicate it to each device → each person just
signs in with their name.**

---

## One-time setup (≈10 minutes) — do this once, then share the file

### 1. Create a free Supabase project
1. Go to **https://supabase.com** → sign in → **New project** (free tier is fine).
2. Give it a name, set a database password, pick a region near you, create it.

### 2. Create the responses table and its access rules
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

-- Tablets (anon key) can only ADD responses. They can never read or delete them.
create policy "anon insert" on public.responses
  for insert to anon with check (true);

-- Only a signed-in admin can READ responses (Results tab + CSV export).
create policy "admin read" on public.responses
  for select to authenticated using (true);
```

### 3. Create the photo bucket
Interview photos are stored as files, not inside the row.

1. In Supabase, open **Storage** → **New bucket** → name it exactly `survey-photos`.
2. Leave it **private** (the admin views photos through short-lived signed links).
3. Back in **SQL Editor**, run:

```sql
-- Tablets can upload photos; only the signed-in admin can view them.
create policy "anon upload photos" on storage.objects
  for insert to anon with check (bucket_id = 'survey-photos');
create policy "admin read photos" on storage.objects
  for select to authenticated using (bucket_id = 'survey-photos');
```

If a photo upload fails (e.g. offline), the image is kept inside the response as a fallback
so nothing is lost.

### 4. Create the admin login
The Results tab asks for an email + password. In Supabase, open **Authentication → Users →
Add user**, create one account for whoever will review results, and tick "auto confirm".
Share that login only with people who should see responses.

### 5. Get your keys
In Supabase: **Project Settings → API**. Copy:
- **Project URL** (looks like `https://abcd1234.supabase.co`)
- **anon public** key (a long string — the *anon* one, NOT the service_role key)

### 6. Paste them into the app
Open `index.html` in any text editor. Near the top, edit the `CONFIG` block:

```js
const CONFIG = {
  SUPABASE_URL: "https://abcd1234.supabase.co",   // your Project URL
  SUPABASE_ANON_KEY: "eyJhbG...your anon key...",  // your anon public key
  TEAM: []            // not used — each surveyor types their own name on the sign-in screen
};
```

Save. That's it — the "not set up" warning disappears once keys are in.

---

## Put it on each tablet
1. Email/AirDrop/USB the `index.html` file to each tablet (same file for everyone).
2. Open it in the browser (Safari on iPad, Chrome on Android).
3. **iPad:** Share → **Add to Home Screen** → it opens full-screen like a real app.
4. Each person types their name once; the app remembers it on that device.

All tablets now write to the **same** database. Open the **Results** tab on any device, sign
in with the admin login, and you see the combined live tally or tap **Download CSV**.

> Tip: keep one "master" copy of `index.html` with your keys already filled in, and copy
> that same file to every tablet.

---

## Results tab (admin)
After admin sign-in the tab shows, top to bottom:
- **Filters** by cluster (business type group) and survey version, with the current *n*
- **Signal tiles** — the headline percentages, including "Already using AI" and
  "Open to focus group (10–20 Dec)"
- **In their words** — every free-text note (pain point, how they use AI) and the interviewer's
  end notes
- **Focus group shortlist & contacts** — one row per response: business, city, owner, phone,
  email, GBP link (opens in a new tab), availability, dates, time slot. Sorted yes → maybe →
  not asked → no
- **Need-gap by job** table and a bar chart for every single question

**↻ Refresh** re-reads the database. **Download CSV** exports everything (see below).

---

## Editing the survey questions
All questions live in the `SECTIONS` array in `index.html`. Each question looks like:

```js
{ id:"role", q:"What is your role?", opts:["Owner","Partner","Manager"] }           // single-select
{ id:"sources", q:"Where do customers come from?", multi:true, opts:[...] }          // multi-select
{ id:"problems", q:"Biggest problems?", multi:true, max:3, opts:[...] }              // multi, max 3
{ id:"age", q:"Your age", opts:[...], noOther:true }                                 // hides the "Other" option
{ id:"ai_how", q:"How are you using AI?", multi:true, cond:COND.usesAI, noteField:"In their words…" }
```

- Give every question a **unique `id`** (used as the database/CSV column).
- `multi:true` = checkboxes (pick many). No `multi` = single pick (auto-advances).
- `noOther:true` removes the "Other → type it" option.
- `cond:COND.xyz` shows the question only when an earlier answer makes it relevant — add the
  rule to the `COND` object. Hidden questions are not required and are dropped from the payload.
- `noteField:"placeholder"` adds a free-text box under the options; its text shows in
  Results under "in their words" and gets its own CSV column.
- `exclusive:["None of these"]` makes that option clear every other pick.
- Add a Hinglish version in the `HI` object (keyed by the same `id`) — if you skip it, the
  English text is shown in both languages.
- New questions are automatically compulsory and automatically appear in the payload, the
  Results bars, the Jump drawer and the CSV — nothing else to wire up.
- Change section emoji/titles/blurbs at the top of each section block (Hinglish titles live in
  `HI._sec`).

---

## Where the data lives
- **Shared:** Supabase table `public.responses` — one row per survey. `payload` (JSON) holds
  everything: every answer, "Other" text, per-question notes, the interviewee details
  (business, city, owner, phone, email, `gbpUrl`), photo reference, price shown, and the
  interviewer's notes. New fields never need a database migration.
- **Photos:** Storage bucket `survey-photos`, one JPEG per response, referenced from the row.
- **On each device (temporary):** a half-finished survey is kept as a draft, and saved responses
  wait in a small offline queue until synced, then are cleared automatically.
- **Export:** Results tab → Download CSV. Columns, in order: saved at, surveyor, version,
  industry, cluster, business, city, owner, phone, email, GBP link, best time, price shown,
  photo ID, photo link (valid 1 hour), then one column per question (grid cells included),
  one column per free-text note, and the interviewer notes.

## Troubleshooting
- **"not set up"** → `CONFIG` keys are blank. Add them (step 6).
- **Can't press Next / Save** → something on the card is still blank. The red message says
  what; the field or options are highlighted. Every question is compulsory by design.
- **Dot stays red / "offline"** → no internet, or wrong URL/key. Responses are still safe on the device and sync later.
- **Results asks for a sign-in** → that's expected; use the admin account from step 4.
- **Nothing in Results after sign-in** → check the table + policies (step 2) ran without error, and you're online. Tap **↻ Refresh**.
- **"permission denied" on save** → the "anon insert" policy in step 2 didn't apply; re-run that SQL.
- **Photo link blank in the CSV** → the bucket in step 3 is missing or its policies didn't apply; the image is still inside the response row as a fallback.

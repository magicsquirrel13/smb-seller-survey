# SMB Seller Survey — Project Handoff

A tablet-first field survey for interviewing metro shop owners. Surveyors ask each
question (English or Hinglish), and responses sync to a shared Supabase database.

---

## 1. What's where

| Piece | Where it lives |
|---|---|
| **The app** (single HTML file) | `smb-survey-app/index.html` |
| **Code repo** | https://github.com/magicsquirrel13/smb-seller-survey |
| **Live link** (for surveyors) | _<< paste your hosted Netlify/GitHub Pages URL here >>_ |
| **Database** (responses) | Supabase project `psybsqqkvmltaqzoczwd` |

---

## 2. Who needs what access

| They will… | Give them access to… | How |
|---|---|---|
| **Run surveys** | Nothing — just the **live link** | Send them the hosted URL. No login needed (surveyors are anonymous). |
| **Edit / deploy the app** | **GitHub repo** | Repo → Settings → Collaborators → Add people → **Write** role |
| **View / download / clear responses** | **Admin login** inside the app | On the **Results** tab, sign in with the admin email + password created in Supabase (Authentication → Users). Only admins can see data. |
| **Manage the Supabase project** | **Supabase** | `supabase.com/dashboard/org/_/team` → Invite member → **Developer** role |

---

## 3. How to deploy a change

1. Edit `index.html`
2. Commit & push to the GitHub repo (or upload the file in GitHub's web UI)
3. The hosting (Netlify / GitHub Pages) auto-rebuilds — the live link updates in ~1 min
4. Reload the app on the tablet to pick up the change

> The responses do **not** live in GitHub — only the app code does. All survey
> answers are stored in Supabase.

---

## 4. How to get the collected data (CSV / Excel)

**Option A — from inside the app:** open the app → **Results** screen → it shows a
summary and can export responses.

**Option B — from Supabase:** Table Editor → `responses` table → export to CSV.

---

## 5. Good to know

- The survey **skips questions that don't apply** (e.g. "Why do you run ads?" only
  appears if the respondent actually runs ads). This is intentional.
- There's an **EN / Hinglish** toggle at the top so surveyors can ask each question
  in either language — the saved data is always stored in English.
- **Viewing, downloading and clearing responses** requires an **admin sign-in** on
  the Results tab (a Supabase login). There is no password in the app's code — it's
  all enforced by the database. Surveyors never see anyone's data.
- The app works offline and queues responses, syncing when back online.

---

_Questions? Contact the project owner (nishtha.singhal@singleinterface.com)._

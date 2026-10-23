# IETS and Water Meter Daily Log

Lam Soon Edible Oils Sdn. Bhd. (Telok Panglima Garang Factory)

A public web page for the IETS and water meter daily log. Anyone with the link can **view** the dashboard, history, trends and reports. Only approved people can **save, edit or delete** readings, after signing in with their email.

- **Website:** `index.html`, hosted free on GitHub Pages
- **Database:** Supabase (project `vzajyuuqnkblgimxpbhf`)
- **Database setup script:** `supabase/setup.sql`

No Google or Claude account is needed to use the page.

---

## Step 1: Set up the database (once)

1. Open <https://supabase.com/dashboard> and open the project.
2. Go to **SQL Editor** → **New query**.
3. Open `supabase/setup.sql`, copy everything, paste it into the editor and press **Run**.
4. The last result should show **logs 77, calib 5, settings 1**.

The script:
- creates the tables
- makes the log readable by anyone and writable only by approved emails
- turns on live updates
- adds the approved people
- loads all existing readings, calibration records and limits

It is safe to run again: it won't create duplicates.

## Step 2: Put the page on GitHub Pages

1. Sign in to <https://github.com> (free account) → **New repository**.
   - Name: `iets-log` (or any name)
   - Visibility: **Public**
   - Tick **Add a README file** → **Create repository**
2. Click **Add file → Upload files**. Drag in `index.html`, this `README.md`, and the `supabase` folder → **Commit changes**.
3. Go to **Settings → Pages**.
   - **Source:** Deploy from a branch
   - **Branch:** `main` / `(root)` → **Save**
4. After about a minute the page is live at:
   `https://<your-github-username>.github.io/iets-log/`

## Step 3: Allow sign-in from the website (once)

In Supabase: **Authentication → URL Configuration**

- **Site URL:** `[https://<your-github-username>.github.io/iets-log/](https://ngaukk.github.io/IETS-Daily-Monitoring-System-Log/)`
- **Redirect URLs:** add the same address → **Save**

Without this step, the email sign-in link won't return people to the log.

## Approved people

| Email | Role |
|---|---|
| ngaukk@gmail.com | owner (can also change Limits) |
| babepopoi07@gmail.com | recorder (Badarudin) |
| sahb85434@gmail.com | recorder (Bishnu) |
| subhaomprakash@gmail.com | recorder (Prakash) |

To add someone, run this in the Supabase SQL Editor:

```sql
insert into public.editors (email, role) values ('new.person@example.com', 'recorder')
on conflict (email) do update set role = excluded.role;
```

To remove someone:

```sql
delete from public.editors where email = 'old.person@example.com';
```

## How people use it

- **Viewing:** open the link. No sign-in needed.
- **Recording:** tap **Sign in** (top right) → enter the approved email → open the link in the email **on the same phone/PC and browser** → the page shows the email and "recorder", and **Save reading** is enabled.

## Notes

- **Free Supabase projects** pause after about a week with no activity. Daily logging keeps the project active. If it ever pauses, open the Supabase dashboard and click **Restore**.
- **Backups:** use **History → Export CSV** regularly, or **Database → Backups** in Supabase.
- **Removed from this version:** photo scan (needs Claude) and Google Sheet/Drive sync. Import CSV from a file still works.
- **The key in `index.html`** is Supabase's *publishable* key. It is meant to be public. Write access is protected by the database rules in `setup.sql`, not by hiding the key.

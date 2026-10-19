# English Paper — Supabase setup

The student login asks for a name only. Under the hood it uses a Supabase anonymous session so each browser session can save its own submissions. This is not identity verification; students can choose another display name.

## 1. Enable anonymous student sign-in

1. Open the Supabase project **Chapter 2**.
2. Go to **Authentication → Sign In / Providers** (the exact menu label may vary).
3. Enable **Anonymous Sign-Ins** and save.
4. Keep Row Level Security enabled on both `english_paper_results` and `english_paper_admins`.

The marks tables and access policies have already been created by the project migration.

## 2. Create the admin account

1. In Supabase, open **Authentication → Users** and create an admin user with an email and a strong password.
2. Copy that user's UUID.
3. Open **SQL Editor** and run the following query, replacing the UUID with the admin user's real UUID:

```sql
insert into public.english_paper_admins (user_id)
values ('PASTE-ADMIN-USER-UUID-HERE')
on conflict (user_id) do nothing;
```

Only accounts listed in `english_paper_admins` can read every student's results or change written marks. Do not put an admin password or a Supabase secret/service-role key in the website code.

## 3. Pages

- Student paper and name login: https://cobra546.github.io/English-test-paper/
- Admin dashboard: https://cobra546.github.io/English-test-paper/admin.html

GitHub Pages may take a little time to publish the newest commit. If Pages is not enabled for this repository, open the repository's **Settings → Pages** and publish from the `main` branch.

## 4. How marks work

- The 20 textbook MCQs and 15 grammar MCQs are auto-marked and saved as **Objective /35**.
- The admin enters **Written /60** after checking the student's written sections.
- **Total /95** appears once written marks are entered.
- Each submit creates a new result row, so repeat attempts are retained.

If the student login says anonymous sign-in is disabled, return to step 1 and enable that setting.

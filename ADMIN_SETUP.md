# English Paper — Student Codes & Admin Setup

## Student login flow

- **New Student — Generate Code:** enter a unique name. Supabase generates a random 10-character code and shows it once.
- **Returning Student — Enter Code:** enter the same name and saved code.
- Save the code somewhere safe. It is not stored in the browser or displayed again after leaving the registration screen.
- Names are unique (case-insensitive). If two students share a name, add a roll number or other identifier to the name.

The database tables, access-code verification RPCs, and Row Level Security policies have already been created in the Supabase project.

## 1. Create the admin account

1. Open Supabase project **Chapter 2**.
2. Go to **Authentication → Users** and create an admin user with an email and a strong password.
3. Copy that user's UUID.
4. Open **SQL Editor** and run this query, replacing the UUID with the admin user's real UUID:

```sql
insert into public.english_paper_admins (user_id)
values ('PASTE-ADMIN-USER-UUID-HERE')
on conflict (user_id) do nothing;
```

Only accounts listed in `english_paper_admins` can read all student results or change written marks. The student access-code flow does not use Supabase anonymous sign-ins.

## 2. Open the pages

- Student paper: https://cobra546.github.io/English-test-paper/
- Admin dashboard: https://cobra546.github.io/English-test-paper/admin.html

GitHub Pages may take a little time to publish new commits. If Pages is not enabled, open the repository's **Settings → Pages** and publish from the `main` branch.

## 3. Marks

- 20 textbook MCQs + 15 grammar MCQs = **Objective /35**, saved when the paper is submitted.
- Admin enters **Written /60** after checking the written sections.
- **Total /95** appears once written marks are entered.
- Every submission is retained as a separate attempt.

## Security notes

- The access code is a secret credential; students should not share it.
- Student names must be unique. A code is required to submit marks under an existing name.
- Never put an admin password or Supabase service-role key in the public website.
- If a student loses their code, an admin should verify the student and arrange a controlled reset rather than making a new account with the same name.

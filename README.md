# Snapshelf

An image-sharing site: accounts, uploads, likes, comments, and a moderator panel.
Frontend is static (`public/`), backend is one Vercel serverless function (`api/index.js`), storage and database are Supabase (free tier).

## Setup

1. **Supabase**: create a project at supabase.com. Open SQL Editor and run `schema.sql`.
2. In Project Settings > API, copy the **Project URL** and the **service_role key** (keep it secret).
3. **Vercel**: push this folder to GitHub, then import it at vercel.com (Framework preset: Other).
4. Add these Environment Variables in Vercel, then deploy:
   - `SUPABASE_URL`: your Project URL
   - `SUPABASE_SERVICE_KEY`: the service_role key
   - `JWT_SECRET`: any long random string (e.g. `openssl rand -hex 32`)
5. Open your site and sign up. Then make yourself a moderator in the Supabase SQL Editor:
   ```sql
   update users set role = 'mod', tag = 'MOD' where username = 'YOUR_USERNAME';
   ```
   Log out and back in. A **Mod panel** link appears: delete images, ban users (1 hour to permanent, reason required), and set any user's tag. Tags show as `[TAG] username` on comments and uploads.

## How it works

- Passwords are hashed with bcrypt. Sessions are signed 7-day tokens; changing your password or using "Sign out everywhere" invalidates every older token.
- Bans are checked on every action. Banned users can browse but cannot upload, like or comment.
- Moderator checks run on the server, so the panel is not just hidden in the UI.
- Images are resized to 1600px and re-encoded as JPEG in the browser (keeps them under Vercel's 4.5 MB request limit and strips EXIF location data).

## Not included yet

Rate limiting, email verification, and password reset by email. Add these before a public launch.

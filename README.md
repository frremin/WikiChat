# ChatApp — a minimal, real WhatsApp-style chat app

Real accounts (signup/login with hashed passwords), a real PostgreSQL database,
and real-time 1:1 messaging over WebSockets. No mock data.

## What's inside
- `server.js` — Express REST API (signup/login/contacts/history) + WebSocket real-time layer
- `schema.sql` / `initdb.js` — database setup
- `public/` — plain HTML/CSS/JS frontend (no build step needed)

## 1. Run it locally

You need Node.js (18+) and a PostgreSQL database (local, or a free hosted one — see step 3).

```bash
cd chatapp
npm install
cp .env.example .env
# edit .env: set DATABASE_URL to your Postgres connection string
npm run initdb    # creates the tables
npm start         # starts the server
```

Then open **http://localhost:3000** in two different browser windows (or one normal +
one incognito) so you can sign up as two different users and message yourself to test it.

## 2. How it works
- Passwords are hashed with bcrypt, never stored in plain text.
- Login issues a JWT stored in the browser's localStorage.
- The WebSocket connection authenticates using that same token (`/ws?token=...`).
- Every message is written to PostgreSQL first, then pushed live to the
  recipient if they're online — so history persists and survives refreshes/restarts.
- The contact list auto-refreshes every 8s so new signups show up without a reload.

## 3. Get a free PostgreSQL database
Pick one (all have free tiers):
- **Neon** — neon.tech — fastest to set up, serverless Postgres
- **Supabase** — supabase.com — Postgres + extras
- **Railway** — railway.app
- **Render** — render.com (also hosts the app itself, see below)

Copy the connection string they give you into `DATABASE_URL` in `.env`.

## 4. Deploy it live (so real people can use it)
Easiest path — **Render**:
1. Push this folder to a GitHub repo.
2. On render.com: New → Web Service → connect the repo.
3. Build command: `npm install`. Start command: `npm start`.
4. Add environment variables `DATABASE_URL` and `JWT_SECRET` in Render's dashboard.
5. Run `npm run initdb` once (Render's Shell tab, or run it locally against the same DATABASE_URL).
6. Deploy — you'll get a public URL anyone can sign up at.

Railway and Fly.io work the same way (connect repo → set env vars → deploy).

## 5. Known limitations (this is v1)
- 1:1 chat only — no group chats yet.
- No read receipts, no message editing/deleting, no media/image messages.
- No password reset flow.
- Typing indicator and online presence are basic (no "last seen").

These are all natural next steps — happy to add any of them next.

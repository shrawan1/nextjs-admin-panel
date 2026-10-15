# Next.js Admin Panel

Full-stack light-theme admin panel using Next.js App Router, TypeScript, Tailwind CSS, PostgreSQL, Prisma, bcryptjs and signed HttpOnly cookie sessions (jose).

## Requirements
- Node.js 20.9+ (Node 22 recommended)
- npm
- Docker Desktop (or a running PostgreSQL server)

## Setup
```bash
cp .env.example .env
# IMPORTANT: replace SESSION_SECRET with a random secret of 32+ characters,
# and change SEED_ADMIN_PASSWORD before running the seed command.
docker compose up -d
npm install
npx prisma generate
npx prisma db push
npm run db:seed
npm run dev
```
Open http://localhost:3000 and sign in with `SEED_ADMIN_EMAIL` and `SEED_ADMIN_PASSWORD` from `.env`.

Generate a secret with `openssl rand -base64 48`.

## Routes
- `/login` — sign in
- `/dashboard` — stats and latest users (signed-in users)
- `/users` — user listing (admins only)
- `/users/new` — create user (admins only)
- `/users/[id]/edit` — update user (admins only)
- Delete user — server action in listing (admins only)

## Notes
- The app uses signed JWT cookies, **not Auth.js**. Authentication is implemented directly with `jose` for a small runnable starter.
- Permissions are checked inside server actions and page loaders, not just in the UI.
- Deleting your own account and demoting your own admin role are blocked.
- Before public production deployment, add login rate limiting, audit logs, password-reset/email verification, session revocation, pagination, and stronger user deletion confirmations. Use a managed PostgreSQL database and HTTPS.
- Database push is intended for local setup. Use Prisma migrations for production schema changes.

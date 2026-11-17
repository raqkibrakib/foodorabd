# Foodora BD — Node.js + Vercel + Supabase Deployment Guide

This project is a full-stack TypeScript application with:
- **Frontend**: React 19 + Vite + Tailwind CSS v4 (`src/`)
- **Node.js Backend**: Express API (`server.ts`)
- **Vercel Serverless Entry**: `api/index.ts` + `vercel.json` (routes `/api/*` to Node.js serverless and all pages to `dist/index.html`)
- **Database ORM**: Drizzle ORM (`src/db/schema.ts`) compatible with **Supabase PostgreSQL** out of the box

---

## 1. Connect Supabase PostgreSQL
1. Create a project at **supabase.com**.
2. Go to **Project Settings -> Database -> Connection string -> URI**.
3. Copy:
   - **Transaction/Session Pooler URL** (port `6543` or `5432`) -> set as `DATABASE_URL` (or `POSTGRES_URL`)
   - **Direct / Session URL** (optional, for `drizzle-kit push`) -> set as `POSTGRES_URL_NON_POOLING`
4. Create a `.env` file from `.env.example` locally and push all 13 tables to Supabase:
   ```bash
   npm install
   npm run db:push
   ```
   Wait for `[✓] Changes applied`. On first server request, `ensureDatabaseSeeded()` automatically seeds Dhaka restaurants, dishes, riders, coupons, and demo orders.

---

## 2. Deploy to Vercel
1. Push the extracted ZIP folder to a GitHub repository (or run `npx vercel`).
2. In **Vercel Project Settings -> Environment Variables**, add:
   - `DATABASE_URL` = your Supabase PostgreSQL URI
   - `SANDBOX_SESSION_SECRET` = any strong random string (32+ chars)
   - `VITE_GOOGLE_MAPS_API_KEY` = your Google Maps Browser API key (optional — built-in Dhaka interactive map fallback works without it)
3. Vercel automatically uses `vercel.json`:
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
   - **Node.js API Serverless Handler**: `api/index.ts` -> `server.ts`
4. After deployment, open **Firebase Console -> Authentication -> Settings -> Authorized Domains** and add your `*.vercel.app` domain so **Sign in with Google** works on production.

---

## 3. Run Locally with Node.js
```bash
npm install
npm run db:push
npm run dev
```
App runs on `http://localhost:3000`.

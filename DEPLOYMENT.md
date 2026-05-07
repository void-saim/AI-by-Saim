# Deploy Superior AI

Superior AI is a dynamic Next.js app. It needs:

- A Node.js hosting platform for the Next.js server.
- A public MySQL-compatible database for users, chats, memories, and history.
- Environment variables for the database and Gemini API key.

## Recommended Free/Low-Cost Setup

Use one of these:

- Vercel for the Next.js app, plus Clever Cloud MySQL DEV plan for the database.
- Render Free Web Service for the Next.js app, plus Clever Cloud MySQL DEV plan for the database.

Your local XAMPP MySQL URL will not work from public hosting because it points to your own computer.

## Environment Variables

Set these in the hosting dashboard:

```env
DATABASE_URL="mysql://USER:PASSWORD@HOST:PORT/DATABASE"
GOOGLE_GENERATIVE_AI_API_KEY="your-google-generative-ai-api-key"
GOOGLE_GENERATIVE_AI_MODEL="gemini-flash-latest"
NEXT_PUBLIC_BASE_PATH=""
```

Keep `NEXT_PUBLIC_BASE_PATH` empty on public hosting so the app runs at the domain root.

For local XAMPP, keep:

```env
NEXT_PUBLIC_BASE_PATH="/wordpress/superior-ai-by-saim"
```

## Database Setup

After setting `DATABASE_URL` to the public MySQL database, run:

```bash
npm install
npm exec prisma -- generate
npm exec prisma -- db push
npm run build
```

## Vercel

1. Push this project to GitHub.
2. Import the GitHub repo in Vercel.
3. In Vercel's project settings, make sure the Root Directory is the folder that contains `package.json`, `app/`, and `pages/`.
4. Add the environment variables above in Project Settings.
5. Deploy.

This repo includes `vercel.json`, so Vercel will use:

```bash
npm run vercel-build
```

That command runs `prisma generate` before `next build`, which avoids stale Prisma Client issues in Vercel's dependency cache.

If Vercel reports `Couldn't find any pages or app directory`, the Root Directory is pointed at the wrong folder. It must be this `superior-ai` project folder, not the WordPress root and not the nested `app` folder.

## Render

Create a Web Service with:

```bash
Build Command: npm install && npm run build
Start Command: npm run start
```

Add the same environment variables in Render.

## Important

- Do not commit `.env`; it contains secrets.
- Free Render web services can sleep when idle.
- Free/dev database plans are for testing, not production.
- Uploads are read in memory and are not stored as files. Chat text and AI answers are stored in MySQL.

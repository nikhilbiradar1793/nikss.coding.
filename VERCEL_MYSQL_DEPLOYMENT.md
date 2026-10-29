# NIKSS Coding Hub — MySQL + Vercel deployment

This version keeps SQLite for local development, and uses MySQL when `DATABASE_URL` is set.
It also adds the PyMySQL driver, quotes migration identifiers safely for MySQL (notably the `user` table), and copies existing static assets to `public/static` for Vercel's static asset serving.

## 1. Create an external MySQL database

Create a MySQL database with a provider that offers a plan suitable for your needs. Copy its connection string and confirm the provider's SSL requirements. Vercel does not provide a built-in MySQL server as part of the Flask deployment.

Set `DATABASE_URL` in the provider's format, for example:

```text
mysql+pymysql://USERNAME:PASSWORD@HOST:3306/DATABASE_NAME?charset=utf8mb4
```

Use the real values from your database provider. URL-encode special characters in the username/password if the provider requires it. If your provider requires SSL options, follow that provider's SQLAlchemy/PyMySQL connection-string instructions.

If your provider supplies a `mysql://...` URL, this app automatically changes the driver prefix to `mysql+pymysql://...`.

## 2. Deploy to Vercel

1. Push the contents of this `nikss_professional` folder to a GitHub repository. `app.py` must be at the repository root, or set Vercel's Root Directory to `nikss_professional` if you keep the parent folder.
2. Import the repository at https://vercel.com/new.
3. Vercel should detect Flask automatically; do not add a Gunicorn start command for Vercel.
4. In **Project → Settings → Environment Variables**, add `DATABASE_URL` for Production and Preview as needed.
5. Add the other required environment variables from `.env.example` (for example `FLASK_SECRET_KEY`, Google OAuth keys, Razorpay keys, and optional Judge0 keys).
6. Redeploy after adding or changing environment variables.

## 3. Important MySQL notes

- On the first successful application start, SQLAlchemy creates missing tables. Existing SQLite records are **not** automatically copied into MySQL. Export/import or write a migration if you need to preserve users, enrollments, payments, courses, or messages.
- Keep a backup before moving production data. Never commit `.env` or expose database credentials.
- If you see `Access denied`, check database username/password and allowed network access.
- If you see `Unknown database`, create the database first and use its exact name.
- If you see `Can't connect` / timeout, check the host, port, SSL settings, and provider network allowlist.
- If you see `No module named pymysql`, confirm `PyMySQL>=1.1.1` is in `requirements.txt` and redeploy.
- If you see a `user` syntax error during a column migration, this version quotes table and column identifiers using SQLAlchemy's dialect preparer.

## 4. Vercel limitations for this application

Vercel runs Flask as a serverless function. Runtime writes to local files are not durable, and uploaded course videos or new logos/posters must not be stored only on the deployment filesystem. The existing upload feature needs external object storage (such as an object-storage provider) to persist newly uploaded media. Large video uploads (the app currently allows 500 MB) are not suitable for Vercel request limits. Keep videos in durable external storage and save their URLs in MySQL before using this as a production learning platform.

## 5. Local development

Without `DATABASE_URL`, the app continues to use local SQLite. To test MySQL locally, set `DATABASE_URL` in `.env` and ensure the database is reachable.

# NIKSS Coding Hub — Website Source

This is the updated Flask website source, including the responsive desktop/mobile navigation and polished typography.

## Included
- Flask application (`app.py`)
- Templates for home, courses, login/register, student dashboard, AI tutor, contact and admin pages
- CSS and JavaScript
- Existing course images and site logo
- Dependency list and environment-variable template

For safety and bundle size, the archive excludes the original `.env` credentials, Git history, local SQLite database and large uploaded course video. Your live database and uploaded video should be backed up separately before replacing a deployed folder.

## Run locally (Windows)

1. Install Python 3.10+.
2. Extract this project folder.
3. Open Command Prompt or PowerShell in this folder.
4. Create and activate a virtual environment:

```powershell
python -m venv venv
venv\Scripts\activate
```

5. Install dependencies:

```powershell
python -m pip install -r requirements.txt
```

6. Copy `.env.example` to `.env` and add your own credentials. Do not share `.env`.
7. Start the site:

```powershell
python app.py
```

8. Open `http://127.0.0.1:5000`.

## Google login and payments

Configure the Google OAuth credentials and Razorpay keys in `.env` and in their respective provider dashboards. Ensure the authorised callback URL matches your local or production domain. Leave optional values blank if you are not using those integrations.

## Navbar updates

- Reduced the oversized NIKSS Coding Hub brand text.
- Added balanced logo/title sizing across desktop, tablet and small mobile screens.
- Improved desktop navigation spacing.
- Retained the existing hamburger overlay menu, close control, escape-to-close and link-click behaviour.
- Added visible keyboard focus styling.

## Before deployment

- Set a strong unique `FLASK_SECRET_KEY`.
- Configure production environment variables in your host dashboard.
- Do not commit `.env`, API keys or credentials.
- Back up your production database and uploaded course videos before deploying.


## Online multi-language compiler

The homepage and navigation include a public Online Compiler for C, C++, Java, Python 3, JavaScript, C#, PHP, and SQL (SQLite). Each run is sent from Flask to the configured Judge0-compatible execution service; submitted code is **not** executed inside the Flask process. Configure `JUDGE0_API_URL` and, if your provider requires authentication, `JUDGE0_API_KEY` in the server environment. The default endpoint is Judge0 CE (`https://ce.judge0.com`) and its availability/usage limits are controlled by that service. For production, use a managed/private Judge0 instance or another authenticated execution provider with rate limits, quotas, and isolated sandboxes.

Open `/compiler` to use the editor. The API endpoint is `/api/compiler/run`. The compiler is independent of course purchases, as requested.


## MySQL and Vercel deployment

See [VERCEL_MYSQL_DEPLOYMENT.md](VERCEL_MYSQL_DEPLOYMENT.md) for the `DATABASE_URL` setup, Vercel deployment steps, common MySQL errors, and important file-upload limitations. Local development still defaults to SQLite when `DATABASE_URL` is not set.

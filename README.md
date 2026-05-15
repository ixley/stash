# Stash

A simple, self-hosted read-it-later app. Save articles, highlights, and Kindle notes to your own database.

**Your data. Your server. No subscription.**

## Features

- **Chrome Extension** - Save pages and highlights with one click
- **Web App** - Access your saves from any device
- **Kindle Sync** - Import highlights from your Kindle library
- **Full-Text Search** - Find anything you've saved
- **Text-to-Speech** - Listen to articles with neural voices (free)
- **iOS Shortcut** - Save from Safari on iPhone/iPad
- **Bookmarklet** - Works in any browser

## Why Stash?

- **Free forever** - Runs on Supabase free tier (500MB, unlimited API calls)
- **You own your data** - Everything stored in your own database
- **No account needed** - Single-user mode, no sign-up friction
- **Works offline** - PWA support for mobile
- **Open source** - Fork it, modify it, make it yours

## Quick Start

1. **Create a Supabase project** (free) at [supabase.com](https://supabase.com)
2. **Run the schema** from `supabase/schema.sql`
3. **Add your credentials** to `extension/config.js` and `web/config.js`
4. **Load the extension** in Chrome (`chrome://extensions` > Load unpacked)
5. **Deploy the web app** to Vercel/Netlify (free)

See [SETUP.md](SETUP.md) for detailed instructions.

## Deploying the Web App

### Vercel (recommended)

1. Push this repo to GitHub (make sure `web/config.js` is in `.gitignore` first — it contains credentials)
2. Go to [vercel.com](https://vercel.com), create a new project, and import your repo
3. Set the **Root Directory** to `web`
4. Click **Deploy** — no build step needed, it's plain HTML/JS

Your app will be live at `https://your-project.vercel.app`.

> **Keep it private**: go to Project Settings → Password Protection and enable it so only you can access the URL.

### Deploying Supabase Edge Functions

The bookmarklet and extension use a Supabase Edge Function to save pages. Deploy it once after initial setup:

```bash
npm install -g supabase
supabase login
supabase link --project-ref YOUR_PROJECT_ID
supabase functions deploy save-page
```

Your project ID is the subdomain in your Supabase URL (e.g. `ibtatmwbvdgzbavtdxrj` from `https://ibtatmwbvdgzbavtdxrj.supabase.co`).

### Local development

```bash
cd web
python3 -m http.server 3000
# Open http://localhost:3000
```

## Project Structure

```
stash/
├── extension/       # Chrome extension
├── web/            # Web app (PWA)
├── tts/            # Text-to-speech generator
├── bookmarklet/    # Universal save bookmarklet
├── ios-shortcut/   # iOS Shortcut for Safari
└── supabase/       # Database schema & Edge Functions
```

## Tech Stack

- **Frontend**: Vanilla JS, HTML, CSS (no framework bloat)
- **Backend**: Supabase (PostgreSQL + REST API)
- **Hosting**: Any static host (Vercel, Netlify, GitHub Pages)

## Security Notes

Stash is designed for personal, self-hosted use. Keep these things in mind:

### Never commit real credentials to a public repository

`extension/config.js` and `web/config.js` contain your Supabase URL, anon key, and user ID. These files are tracked by git with placeholder values — once you fill in your real credentials, **do not push them to a public repo**. Anyone with your anon key and project URL can read and write your data.

To protect yourself, add your filled-in config files to `.gitignore` before committing:

```
# Add to .gitignore after filling in real credentials
extension/config.js
web/config.js
```

### Keep the app private

The web app runs in single-user mode using a hardcoded user ID — there is no login screen protecting it. Whoever can reach the URL can see your data. Deploy to a private URL, or put it behind a VPN, Tailscale, or password-protected hosting (e.g. Vercel password protection).

### Supabase project settings

- Keep your Supabase project's **anon key** out of public repos (see above).
- Your Supabase project is protected by Row Level Security (RLS) policies from `schema.sql` — do not disable these.
- If you use the TTS feature, audio files are stored in a Supabase storage bucket. By default this bucket may be public; check your Supabase dashboard under Storage and set it to private if you want audio files to be non-discoverable.

## Screenshots

*Coming soon*

## License

MIT

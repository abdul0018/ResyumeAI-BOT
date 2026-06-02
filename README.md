# Rezyume AI — Vercel deploy

Mini App (static HTML) + Telegram bot (serverless functions) in one Vercel project.

```
.
├── public/
│   ├── index.html        # the Mini App (served at /)
│   └── rezyume-ai.html   # same file, also at /rezyume-ai.html
├── api/
│   ├── telegram-webhook.js  # /start → welcome + "Boshlash 🚀" button
│   ├── send-pdf.js          # delivers generated PDF to the chat
│   └── setup.js             # one-time: registers webhook + menu button
├── vercel.json
└── package.json
```

---

## 0. Revoke the leaked token first
You shared a token in chat — revoke it: **@BotFather → your bot → API Token → Revoke**.
Use the NEW token only as a Vercel env var below.

## 1. Deploy
Two options.

**A) Vercel CLI**
```bash
npm i -g vercel
cd this-folder
vercel            # follow prompts → gives you https://<project>.vercel.app
vercel --prod     # production deploy
```

**B) GitHub + Vercel dashboard**
Push this folder to a GitHub repo → Vercel → "New Project" → import the repo →
Deploy. No build step needed (it's static + functions).

## 2. Set environment variables
Vercel → Project → **Settings → Environment Variables** (Production), add:

| Name        | Value                                              |
|-------------|----------------------------------------------------|
| BOT_TOKEN   | your NEW token from BotFather                       |
| MINIAPP_URL | `https://<project>.vercel.app/`                     |
| PUBLIC_URL  | `https://<project>.vercel.app`                      |

Then **redeploy** so the functions pick up the vars (`vercel --prod`, or "Redeploy" in the dashboard).

## 3. Register the bot (one time)
Open in a browser:
```
https://<project>.vercel.app/api/setup
```
You should see `{"ok":true,...}`. This sets the webhook AND the
"Open Rezyume AI" menu button.

## 4. Test
In Telegram, send **/start** to your bot →
welcome message + **Boshlash 🚀** button appears → tapping it opens the Mini App.

---

## Notes
- The Mini App calls `/api/send-pdf` on the same domain — no extra config.
- `MINIAPP_URL` must end with `/` (or point to `/rezyume-ai.html`); both work.
- To change the welcome text, edit `api/telegram-webhook.js` and redeploy.
- Telegram requires HTTPS for Mini Apps — Vercel provides it automatically.

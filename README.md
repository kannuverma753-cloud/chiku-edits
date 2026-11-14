# Chiku Edits — Portfolio + AI Assistant

## Folder
```
chiku-edits/
├── index.html      ← poori website + "Ask Chiku AI" chat widget (frontend)
├── api/chat.js     ← secure backend route  POST /api/chat  (Vercel serverless function)
├── .env            ← API key (sirf aapke computer pe — GitHub pe kabhi mat daalna)
├── .env.example    ← template (bina key ke)
├── .gitignore      ← .env ko GitHub se bahar rakhta hai
└── package.json
```

## Kaise kaam karta hai
Browser  →  `/api/chat` (aapka server)  →  Google Gemini API
API key sirf server pe `process.env.API_KEY` se padhi jaati hai. Website ke code mein key kahin nahi hai.

## Live karna (Vercel — free)
Netlify Drop pe sirf HTML chalta hai, backend nahi. Isliye chatbot ke liye **Vercel** use karo.

### Option A — GitHub se (sabse aasaan)
1. GitHub pe naya repo banao aur ye poora folder upload karo (`.env` upload NAHI hogi, `.gitignore` use rok degi — ye sahi hai).
2. vercel.com pe login karo → **Add New → Project** → apna repo import karo → **Deploy**.
3. Vercel project → **Settings → Environment Variables** mein add karo:
   - `API_KEY` = aapki Gemini key
   - `GEMINI_MODEL` = `gemini-3.8-flash` (optional)
4. **Deployments → Redeploy** dabao. Bas — website + chatbot live!

### Option B — Vercel CLI se
```bash
npm i -g vercel
cd chiku-edits
vercel            # pehli baar login + deploy
vercel env add API_KEY        # key paste karo (Production)
vercel --prod
```
Local test: `vercel dev` → http://localhost:3000 (ye `.env` file se key padhta hai).

## Badalna ho to
- Bot ki personality / info → `api/chat.js` mein `SYSTEM_PROMPT`
- Model → env var `GEMINI_MODEL`
- Phone / email / WhatsApp → `index.html` mein `const CONFIG={ ... }`

---

## Netlify pe live karna (chatbot ke saath)
Backend file: `netlify/functions/chat.mjs` (ye `/api/chat` pe chalta hai — website ko kuch badalne ki zaroorat nahi).

> ⚠️ Netlify **Drop** (drag & drop) pe sirf website chalegi, chatbot NAHI — kyunki drag & drop deploy mein backend functions nahi chalte. Chatbot ke liye GitHub wala tareeka use karo.

1. GitHub pe naya repo banao → poora folder upload karo (`.env` chhod ke; `api` aur `netlify` dono folders zaroor upload karna).
2. app.netlify.com → **Add new site → Import an existing project → GitHub** → apna repo chuno.
3. Build settings khali chhod do (netlify.toml sab set kar deta hai) → **Deploy**.
4. **Site configuration → Environment variables → Add a variable**: Key `API_KEY`, Value = aapki Gemini key.
5. **Deploys → Trigger deploy → Deploy site** dabao. Bas — website + chatbot live!

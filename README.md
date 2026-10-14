# W² Ai — AI-enabled educational website

This package preserves the original single-page W² Ai interface and connects its chat assistant, research-question helper, and AI-generated 50-question MCQ tests to the Gemini API through a server-side endpoint.

## What changed

- Replaced the Claude-artifact-only `window.claude.use("sample")` integration with calls to `/api/ai`.
- Added `api/ai.js`, a server-side Gemini API proxy. The API key stays in the server environment and is never sent to the browser.
- Chat, research prompts, and JSON MCQ generation use the same backend.
- Added basic request validation and a best-effort per-instance rate limit.
- Kept the original page styling and client-side navigation.

## Deploy online (recommended: Vercel + Gemini free tier)

1. **Get a Gemini API key.** Visit https://aistudio.google.com/app/apikey, sign in, and create an API key. Check the current free-tier availability and limits at https://ai.google.dev/gemini-api/docs/pricing.
2. **Create a GitHub repository.** Sign in to https://github.com, create a new repository (for example `w2-ai`), and upload all files/folders from this project. `index.html` must be at the repository root and `api/ai.js` must be inside the `api` folder.
3. **Import into Vercel.** Visit https://vercel.com, sign in with GitHub, choose **Add New → Project**, import the `w2-ai` repository, and deploy. Vercel serves `index.html` and exposes `api/ai.js` as a serverless function.
4. **Add the secret key.** In Vercel open your project → **Settings → Environment Variables**. Add `GEMINI_API_KEY` with the value from AI Studio. Optionally add `GEMINI_MODEL` with `gemini-2.5-flash`. Select Production (and Preview if you want preview deployments), save, then redeploy from **Deployments → Redeploy** so the new variables take effect.
5. **Test the deployed URL.** Open it on your phone and computer. Register/login, open **AI Chat**, send a simple question, open a subject and use **Generate research questions with AI**, then open a quiz and click **New 50-Mark AI Test**.

## Run locally (optional)

Requires Node.js and a terminal. In this folder:

```bash
npm install
npx vercel login
npx vercel dev
```

When prompted, link/create a Vercel project. Add `GEMINI_API_KEY` to the linked project's environment variables, or create a local `.env.local` file containing:

```env
GEMINI_API_KEY=your_real_key_here
GEMINI_MODEL=gemini-2.5-flash
```

Do not upload `.env.local` to GitHub. Then open the local URL printed by `vercel dev` (typically http://localhost:3000). Do not double-click `index.html` directly; that bypasses the `/api/ai` server endpoint.

## Notes and limitations

- The Gemini API free tier has model-specific rate limits and availability; it is not unlimited, and requests can temporarily fail when limits are reached.
- The research assistant can help plan research and suggest keywords/methodologies, but this package does **not** add live web search or verify sources in real time. Confirm references through scholarly databases before citing them.
- Existing account registration is browser-local (localStorage), not secure multi-user authentication. Do not use it for sensitive data or as a production identity system without adding a real auth/database service.
- The in-memory rate limiter is best-effort on serverless hosting, not a complete abuse-prevention system. For a public production launch, consider a persistent rate limit and authentication.
- Avoid sending private, confidential, or sensitive information to the AI provider. Check provider terms and current API data-use policy.

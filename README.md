# Full AI StoryMotion Studio — Free-tier APIs / Render

Node.js + Express app for Burmese story generation, AI scene images, Burmese neural narration, storyboard JSON and browser-based WebM export.

## API providers

- **Story / storyboard:** Google Gemini API (`gemini-2.5-flash`). Free-tier eligibility, quotas and model availability depend on your account and region. Create a key at https://aistudio.google.com/apikey.
- **Images:** Cloudflare Workers AI `@cf/black-forest-labs/flux-1-schnell`. Cloudflare documents a free allocation of 10,000 Neurons/day; requests stop or may require a paid plan if limits are exceeded. Create an account and an API token with Workers AI permissions at https://dash.cloudflare.com/.
- **Burmese voice:** Azure AI Speech Neural TTS. Uses `my-MM-NilarNeural` (female) and `my-MM-ThihaNeural` (male). Azure lists a Free F0 allowance of 0.5 million Neural TTS characters/month, subject to service quotas and account eligibility. Create a Speech resource at https://portal.azure.com/ and copy its key and region.

Free tiers have quotas and can change. Azure may ask you to create a resource or verify an account; check the current pricing/terms before production use.

## Deploy from Android using GitHub + Render

1. Extract this ZIP.
2. Create a private repository at https://github.com/ and upload the project contents while preserving the `public/` folder. Never upload `.env` or real API keys.
3. In Render, create a Blueprint from the repository (or a Node Web Service with build command `npm install` and start command `npm start`).
4. In Render > your service > Environment, set all of these values:
   - `GEMINI_API_KEY` — Google AI Studio key
   - `GEMINI_MODEL` — `gemini-2.5-flash` (default)
   - `CLOUDFLARE_ACCOUNT_ID` — Cloudflare account ID
   - `CLOUDFLARE_API_TOKEN` — Cloudflare API token with Workers AI permissions
   - `AZURE_SPEECH_KEY` — Azure Speech resource key
   - `AZURE_SPEECH_REGION` — resource region, e.g. the region shown in Azure Portal (not the display name)
5. Save and redeploy. Open `https://YOUR-SERVICE.onrender.com/api/health`; it should show `ok: true` and `services.story/image/voice: true` once all keys are set.
6. Open the main Render URL in Chrome. Use “Add to Home screen” if you want an app-like shortcut.

## Local test

Requires Node.js 20+.

```bash
cp .env.example .env
# Edit .env and add the real keys
npm install
npm start
```

Then open http://localhost:3000.

## Troubleshooting

- **Gemini 400/403/429:** check the key, selected model, account eligibility, region, and free-tier quota.
- **Cloudflare 401/403:** verify account ID and token permissions. **429 / quota error:** check Workers AI usage and daily free allocation.
- **Azure Speech 401/403:** key and region must belong to the same Speech resource. The voice names are `my-MM-NilarNeural` and `my-MM-ThihaNeural`.
- `/api/health` reports which services have environment variables set; it does not guarantee that external keys are valid.
- Render free web services may sleep after inactivity.
- This starter creates still images and applies pan/zoom motion; it is not full frame-by-frame character animation.
- Keep all API keys in Render Environment, never in frontend HTML, GitHub commits, screenshots or chat messages. Add authentication and usage limits before sharing publicly.

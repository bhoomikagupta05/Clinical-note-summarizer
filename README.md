# Medi Scribe X Setup and Deployment

## Requirements

- Node.js 22 or newer
- npm
- Google AI Studio API key

## Local Setup

1. Install dependencies:

```bash
npm install
```

2. Configure environment variables in `.env`:

```bash
GEMINI_API_KEY=your_google_ai_studio_api_key
```

3. Start the development server:

```bash
npm run dev
```

4. Open the app:

```text
http://127.0.0.1:8080/
```

## Useful Scripts

```bash
npm run dev
```

Runs the local Vite/TanStack Start development server.

```bash
npm run lint
```

Runs ESLint. Current known warnings are React Fast Refresh export warnings from shared UI files.

```bash
npm run build
```

Builds the app for production.

```bash
npm run preview
```

Previews the production build locally.

## AI Provider

The app uses Google AI Studio Gemini through:

```text
GEMINI_API_KEY
```

The server function lives in:

```text
src/gemini.functions.ts
```

It calls the Gemini `generateContent` endpoint and asks the model to return strict JSON for the clinical report.

## PDF Export

The report page can export the latest generated report as a PDF from the browser. The export implementation is in:

```text
src/routes/_app.report.tsx
```

The browser first tries the native save-file dialog. If unavailable, it falls back to a normal browser download.

## Deployment Notes

This project is configured for Cloudflare through:

```text
wrangler.jsonc
```

Before deployment, set the production secret in Cloudflare rather than committing it:

```bash
wrangler secret put GEMINI_API_KEY
```

Then build:

```bash
npm run build
```

Deploy using your Cloudflare/TanStack Start deployment flow.

## Security Notes

- `.env` is included in this handoff zip because it was explicitly requested.
- Do not commit `.env` to Git.
- `.gitignore` excludes `.env`, `.env.*`, generated build output, and dependencies.
- Rotate API keys before sharing the zip with anyone else.

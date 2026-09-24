# AI Google Ads Manager — Web App

A single-page web app for the multi-tenant, multi-account AI Google Ads Manager that runs on n8n.

- **Dashboard** — spend, conversions, CPA vs target, zero-conversion spend, daily trend, campaign table with impression-share losses, open issues.
- **Recommendations** — review AI recommendations with rationale, pros and cons; approve or decline. Approved changes are re-validated (ownership, permission, current state) before anything is sent to Google Ads.
- **AI Chat** — ask about performance, waste, cross-campaign overlap or cross-account comparisons, or request a change (it becomes a proposal you approve).
- **Change log** — every execution attempt: applied, verified, no-op, blocked, failed.

## Sign in
Use your **Tenant ID**, **User ID** and personal **API key** issued by the platform. The key is checked server-side (sha256) on every request; the app never stores it unless you tick "Keep me signed in".

## Backend
Static site, no build step. It calls two n8n webhooks on the configured server (default `https://n8n.assignover.in`):

| Webhook | Purpose |
|---|---|
| `POST /webhook/gads-webapp-api` | `action=bootstrap` (dashboard data scoped to the user) and `action=decide` (approve / decline) |
| `POST /webhook/gads-ai-manager` | AI chat (master controller) |

Requests are sent as `application/x-www-form-urlencoded` so browsers do not need a CORS preflight.

No secrets live in this repository.

# Waitlist — BaseHub-powered Waitlist Template

A fully featured **waitlist landing page** for startups and indie hackers collecting early-adopter signups. Built on the official [BaseHub waitlist template](https://basehub.com/templates): BaseHub acts as the CMS (all copy, feature blocks and the waitlist itself are editable in the BaseHub dashboard), Resend sends transactional and newsletter emails, and Next.js renders it all.

## Features

- **Waitlist signup form** (`components/waitlist-form/`) — email capture with idle/loading/success/error states, wired to BaseHub
- **Mesh-gradient hero** (`components/mesh-gradient.tsx`) and dark/light-aware imagery (`dark-light-image.tsx`)
- **Manifesto page** (`app/manifesto/page.tsx`) — long-form brand story page
- **Newsletter emails** (`emails/newsletter/index.tsx`) — React Email templates rendered and sent via Resend
- **Email unsubscribe route** (`app/api/email-unsubscribe`) — one-click unsubscribe handling
- **Post-created webhook** (`app/api/webhooks/post-created`) — revalidates pages when BaseHub content changes
- **Theme switcher** (`components/switch-theme`) with `next-themes` provider
- **Playground notification** — BaseHub playground banner component

## Tech stack

- **Framework:** Next.js 15.2.4 (App Router), React 19
- **CMS:** BaseHub (`basehub` SDK, `basehub.config.ts`, generated `basehub.d.ts`)
- **Email:** Resend + react-email templates
- **Styling:** Tailwind CSS 3.4, Radix UI primitives, `lucide-react`, `@paper-design/shaders-react` for shader effects
- **Type safety:** TypeScript

## Quick start

```bash
# install dependencies
pnpm i        # or: npm install

# configure environment (see below)
cp .env.example .env.local   # if present, else create .env.local manually

# start the dev server
pnpm dev      # -> http://localhost:3000

# production build
pnpm build
pnpm start
```

## Environment variables

Both are **required** — the app cannot build or run meaningfully without them:

| Variable | Where to get it | Purpose |
|---|---|---|
| `BASEHUB_TOKEN` | BaseHub dashboard → your repo → API token | Fetches CMS content (copy, waitlist config); blocks signup submissions if missing |
| `RESEND_API_KEY` | [resend.com](https://resend.com) API keys | Sends welcome/newsletter emails and powers unsubscribe links |

```txt
# .env.local
BASEHUB_TOKEN="<your-basehub-token>"
RESEND_API_KEY="<your-resend-api-key>"
```

> Note: never commit `.env.local` — it is already listed in `.gitignore`.

## Project structure

```
waitlist/
├── app/
│   ├── api/
│   │   ├── email-unsubscribe/route.tsx   # Unsubscribe link handler
│   │   └── webhooks/post-created/route.tsx # BaseHub revalidation webhook
│   ├── manifesto/page.tsx               # Brand manifesto page
│   ├── globals.css                      # Tailwind + global styles
│   ├── layout.tsx                       # Root layout
│   └── page.tsx                         # Landing page (fetches content from BaseHub)
├── basehub.config.ts                    # BaseHub repo config
├── basehub.d.ts                         # Generated BaseHub types
├── components/
│   ├── box/                             # Layout primitives
│   ├── header/                          # Site header
│   ├── switch-theme/                    # Dark/light toggle
│   ├── waitlist-form/                   # Signup form with state machine
│   ├── dark-light-image.tsx             # Theme-aware image component
│   ├── mesh-gradient.tsx                # Animated gradient background
│   ├── playground-notification.tsx      # BaseHub playground banner
│   └── theme-provider.tsx               # next-themes wrapper
├── context/index.tsx                    # App context providers
├── emails/newsletter/index.tsx          # Newsletter email template (react-email)
├── lib/
│   ├── resend/index.ts                  # Resend client setup
│   └── utils.ts                         # cn() helper
├── assets/dots.tsx                      # Decorative SVG dots
├── public/                              # Static assets (logos, placeholders)
└── styles/                              # Additional styles
```

## Deployment notes

- This is a **serverful Next.js app** — API routes, BaseHub fetching at request time, and email sending need a Node runtime plus the two secret env vars, so it does **not** work as a static export. Deploy where server features are available (Vercel one-click template deploy works best; any Next.js-capable host works too).
- The BaseHub Vercel integration wires `RESEND_TOKEN` automatically via the "Deploy with Vercel" flow if you start from the template.

---

Built by Girish Lade — [ladestack.in](https://ladestack.in)

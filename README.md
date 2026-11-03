# Wekaro

**Learn AI engineering, week by week.** Wekaro is a gamified 26-week study platform that takes self-taught developers from AI fundamentals to production LLM systems, with a dedicated exam sprint for the Claude Certified Architect certification. It replaces scattered bookmarks and half-finished courses with one structured roadmap, timed quizzes, and visible progress (XP, streaks, badges) that keeps learners coming back.

[![Next.js](https://img.shields.io/badge/Next.js-16-000000?logo=nextdotjs&logoColor=white)](https://nextjs.org)
[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-06B6D4?logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![Supabase](https://img.shields.io/badge/Supabase-Auth_%2B_Postgres_RLS-3FCF8E?logo=supabase&logoColor=white)](https://supabase.com)
[![Paystack](https://img.shields.io/badge/Paystack-Payments-0BA4DB?logo=paystack&logoColor=white)](https://paystack.com)
[![Framer Motion](https://img.shields.io/badge/Framer_Motion-12-0055FF?logo=framer&logoColor=white)](https://www.framer.com/motion/)
[![Vercel](https://img.shields.io/badge/Deployed_on-Vercel-000000?logo=vercel&logoColor=white)](https://vercel.com)

> Independent project. Not affiliated with Anthropic.

---

## Architecture

```
                         ┌──────────────────────────────┐
  Visitor (browser) ───▶ │  proxy.ts (Next.js middleware)│
                         │  public / auth / paid routes  │
                         └──────────────┬───────────────┘
                                        │
             ┌──────────────────────────┼───────────────────────────┐
             ▼                          ▼                           ▼
   Public hub + /courses        Paid app (roadmap,            API routes
   (statically generated)       quizzes, badges, profile)     /api/checkout
                                        │                     /api/webhooks/paystack
                                        ▼                     /api/redeem-invite
                          ProgressProvider (React context)    /api/subscribe
                                 │            │                     │
                    instant write│            │fire-and-forget      │
                                 ▼            ▼ upsert              ▼
                           localStorage   Supabase Postgres  ◀── service-role
                           (offline cache) user_progress JSONB    client
                                          purchases, invite_codes
                                          (row-level security)
                                                ▲
  Paystack checkout ──▶ charge.success webhook ─┘ (HMAC-verified → mark_user_paid)
```

**Request flow, in short:**

1. Every first visit gets a Supabase **anonymous session**, so progress is saved before anyone signs up.
2. `src/proxy.ts` sorts each route into public, signed-in, or paid, and reads the paid flag from the JWT's `app_metadata`.
3. Progress writes go to `localStorage` first (instant UI), then sync to a per-user JSONB row in Supabase.
4. Payments go through Paystack. A signature-verified webhook records the purchase and flips the user's paid flag, which unlocks the app on the next token refresh.

## Key Features

### Learning engine
- **26-week roadmap** across 5 phases: AI Foundations, Developer Skills, Architect Exam Sprint, Production AI & LLMOps, Portfolio & Career. Every resource is deep-linked and typed (course, docs, video, build, quiz).
- **Quiz engine** with a timer, question navigation, scoring, and a per-domain breakdown. It covers 5 domain quizzes, weekly quizzes, and scenario-based capstones (100+ questions).
- **Practice exam simulator** that draws 30 questions at random, weighted to the real exam's domain split (27 / 19 / 19 / 20 / 15%), with a 45-minute limit and a 72% pass mark.

### Gamification
- **XP by effort type**: a build project is worth more than reading docs. Streak multipliers apply (1.5x at 7 days, 2x at 30).
- **10 levels** with a full-screen level-up celebration, **19 badges** in 4 categories (streak badges are earned automatically), a study heatmap, and weekly reflection check-ins.

### Platform engineering
- **Anonymous to real account without losing data.** Sign-up calls `updateUser` on the anonymous session, and Google sign-in uses `linkIdentity`, so the user keeps the same UUID and every bit of progress.
- **Hybrid offline-first storage:** synchronous `localStorage` reads for an instant UI, with Supabase as the durable source of truth.
- **Route-level access control** in `proxy.ts`, using cookie-based Supabase SSR sessions so the server can read the session.
- **Payments:** Paystack one-time checkout (NGN / USD), an HMAC-SHA512 verified webhook, an idempotent `purchases` table (unique on reference), a post-redirect verification fallback with retries, and invite codes.
- **Full auth flows:** email confirmation with resend, forgot and reset password, and Google OAuth.

### Built with AI, for AI engineers
The roadmap, quiz bank, and practice exam focus on the skills the Claude Certified Architect exam tests: agentic architecture, tool integration and MCP, Claude Code, prompt engineering, and context management. The codebase was built pair-programming with Claude Code, and the project-level `CLAUDE.md` / `AGENTS.md` files are the agent instructions used during development.

## Project Structure

```
src/
├── app/                  # Next.js App Router pages + API routes
│   ├── (auth)/           # login, signup, forgot/reset password
│   ├── api/              # checkout, paystack webhook, verify-payment, redeem-invite, subscribe
│   ├── courses/          # public, statically generated course pages
│   ├── roadmap/ quiz/ badges/ profile/ check-ins/   # paid app
├── components/
│   ├── hub/              # public marketing site
│   ├── shared/           # progress provider, header, confetti, level-up overlay
│   └── ui/               # shadcn/ui primitives
├── lib/
│   ├── data/             # roadmap, quiz questions, courses (typed content)
│   ├── gamification/     # XP, badge, quiz and practice-exam engines
│   ├── store/            # localStorage store + Supabase sync
│   ├── supabase/         # browser/server clients, generated DB types, purchases
│   └── paystack.ts       # transaction init, verify, webhook signature check
└── proxy.ts              # route protection (Next.js 16 middleware)
```

## Getting Started

**Requirements:** Node.js 20+ and npm. For auth, progress sync and payments you also need a Supabase project and (optionally) a Paystack account.

```bash
git clone https://github.com/Sirjamnesy/Wekaro.git
cd Wekaro
npm install
cp .env.example .env.local   # then fill in your own keys
npm run dev                  # http://localhost:3000
```

Other scripts:

```bash
npm run build   # production build
npm run start   # serve the production build
npm run lint    # ESLint
```

### Environment variables

See [`.env.example`](.env.example). Only the two `NEXT_PUBLIC_SUPABASE_*` values are required to boot the app. The service-role and Paystack keys are server-only and must never be exposed to the browser.

| Variable | Required | Used for |
|---|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Yes | Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Yes | Public anon key (RLS enforced) |
| `SUPABASE_SERVICE_ROLE_KEY` | For payments / sign-ups | Server-only writes (webhook, invite codes, email list) |
| `PAYSTACK_SECRET_KEY` | For payments | Initialize + verify transactions, verify webhook signatures |
| `NEXT_PUBLIC_APP_URL` | Recommended | Absolute URLs for payment callbacks and share cards |

## Deployment

The app deploys on Vercel. Push to `main` to deploy, and set the variables above in the Vercel project settings. For live payments, point the Paystack webhook at `https://<your-domain>/api/webhooks/paystack`.

## License

All rights reserved. The source is visible for portfolio purposes.

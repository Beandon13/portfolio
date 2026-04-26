# saige2.0: AI tutor with hierarchical long-term memory

## One-line summary

Production AI tutor for college students built on a Next.js monorepo, with a hierarchical long-term memory system that combines pgvector embeddings, full-text search, and conversation auto-compaction.

## What it does

saige2.0 gives each student a course-aware AI tutor with persistent memory across sessions. Students upload course PDFs into a folder hierarchy (course, then unit, then lesson), then chat with a Gemini-powered tutor that retrieves the relevant slice of their course content and conversation history on every turn. Free trial with no credit card; paid tier for unlimited courses. Live in production on Vercel and Supabase.

## The problem

Most AI tutors throw the entire transcript and a few PDFs at the model and hope for the best. That breaks down once a student has multiple courses, weeks of conversations, and hundreds of pages of material. saige2.0 solves the retrieval problem at production scale: how to keep responses grounded in the right course content and the right past conversations, without blowing the context window or burning the user's wait time on every message.

## My role

Contributing engineer on a production team. I shipped work across the stack and across the LTM system itself: improvements to the dashboard context retrieval pipeline, a hybrid-search recall fix, pgvector and SQL-function migration debugging, the in-app Stripe cancellation flow, the email-verified page rewrite that fixed a silent onboarding failure, the PostHog analytics and session-recording integration, document-upload timeout handling, and a full mobile responsiveness overhaul (bottom nav, chat drawer, full-width layouts). Roughly 50 commits across `apps/web`, `supabase/migrations`, and `packages/types`.

## Technical scope

- **Monorepo**: pnpm 9 plus Turborepo, with a Next.js 16 web app and shared `packages/types` (auto-generated from the Supabase schema) and `packages/database` (typed Supabase clients).
- **Backend**: Next.js API routes for chat, LTM, content, courses, checkout, and webhooks.
- **Database**: Supabase Postgres with pgvector and ltree extensions. Versioned migrations. Separate staging and production projects with GitHub Actions auto-deploying migrations on branch push.
- **AI**: Google Gemini via the Vercel AI SDK, with streaming responses and Gemini text embeddings (768-dim) for retrieval.
- **Background jobs**: Upstash QStash for fire-and-forget embedding generation and conversation compaction. Trial expiration cron.
- **Billing**: Stripe subscriptions with database-backed trial metadata.
- **Auth**: Supabase Auth with cookie-based SSR and RLS-enforced data access.
- **Analytics**: PostHog with session recordings; Vercel Analytics.

## Key engineering work

- **Dashboard context retrieval improvements.** Worked on the LTM pipeline that assembles per-turn context out of recent messages, the hierarchical course/folder path, semantic matches inside the current subtree, semantic matches across the entire course, and rolled-up conversation summaries. Tightened budgets and recall behavior so the right context survives compression.
- **Hybrid search recall fix.** Diagnosed and fixed full-text search recall in the hybrid retriever, plus a pgvector migration bug where the `vector` type wasn't resolving inside a SQL function. Fix: explicit `SET search_path` so the function could see the extension.
- **Migration and SQL function debugging.** Shipped forward-compatible fixes for migration ordering and stored-procedure evolution: function recreations across return-type changes, UUID generator swaps, missing-column patches in copy functions. Never rewriting history.
- **In-app Stripe cancellation flow plus subscription UX.** Built the in-product cancel surface so users don't have to leave for the Stripe portal, paired with the email-verified page rewrite that fixed a silent onboarding failure where signups were getting stuck after verification.
- **PostHog analytics and session recordings.** Wired PostHog into the app (correct default-import handling, env-var trimming, recording opt-in) and set up the dashboard for product analytics.
- **Mobile responsiveness overhaul.** Re-laid the chat surface for mobile: bottom nav, slide-out chat drawer, full-width layouts. Removed the desktop-only sidebar from the class layout on mobile so the chat surface actually fits the screen.
- **Document upload reliability.** Added timeout handling to document uploads so a slow or stalled extraction couldn't hang the upload UI indefinitely.

## Challenges and constraints

Two recurring constraints: retrieval quality under a token budget, and shared-codebase coordination.

On retrieval, you can't stuff everything into the model. The pipeline has to decide what to include from recent messages, the hierarchical path, local subtree similarity, global course similarity, and rolled-up summaries, all under a budget that leaves room for the response. Improving recall without blowing the budget is a constant tradeoff.

On the codebase, production migrations move forward only. Debugging a function-return-type change or a missing column in a stored procedure means writing a forward-compatible patch rather than rewriting history. Several of my commits are exactly that pattern: drop, recreate, fix forward.

## Outcomes

- Live in production on Vercel and Supabase Cloud, with staging-first GitHub Actions deployments.
- Shipped end-to-end work on the LTM and hybrid-search system, including a real pgvector migration fix that blocked a SQL function from working correctly.
- In-app Stripe cancel flow shipped, tied to the email-verified rewrite that fixed a silent onboarding failure.
- Mobile experience reworked from a desktop-first layout to a mobile-native chat surface.
- PostHog session recording and analytics fully integrated.
- Multiple database-function and migration bugs diagnosed and patched without breaking forward compatibility.

## Why it matters for contract work

This is the kind of work AI-application teams actually need: not a notebook RAG demo, but the production-grade plumbing that makes a memory-augmented tutor work at scale. The retrieval pipeline, the migration discipline, the in-app billing flows, and the mobile chat surface are all real-world systems that have to keep working when paying users hit them. Showing up to a production codebase, finding the bug in a SQL function, and shipping a forward-compatible fix is the freelance/contract skillset, not the demo skillset.

## Public-safe summary

saige2.0 is a production AI tutor for college students, built on a Next.js + Supabase + Gemini monorepo with a hierarchical long-term memory system (pgvector embeddings, full-text hybrid search, conversation auto-compaction, QStash background jobs). I contributed across the stack: improvements to the dashboard context retrieval pipeline, pgvector and migration fixes, the in-app Stripe cancellation flow, PostHog analytics integration, a mobile responsiveness overhaul, and onboarding/auth UX fixes. Live on Vercel and Supabase Cloud with staging-first GitHub Actions deployments.

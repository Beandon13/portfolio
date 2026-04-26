# Brian Zarnitz

AI engineer available for freelance and contract work in agent engineering, LLM and agent evaluation, AI safety / red teaming, and edtech engineering.

## Who I am

Over a year of AI code evaluation and RLHF-style experience, currently studying CS at Binghamton University. I ship production-shaped systems, not notebook demos: multi-tenant agent platforms, retrieval-augmented tutoring, hardened LLM endpoints, and the SaaS plumbing that has to work when paying users hit it.

## What I do

- Design and build AI agent systems on real infrastructure
- Harden LLM endpoints against prompt injection and provider failure
- Build and improve retrieval pipelines (pgvector, full-text hybrid, conversation compaction)
- Wire payments, auth, and webhooks correctly the first time
- Evaluate model behavior across providers and surface regressions

## Featured projects

### HubClaw: Personal AI agent platform

A SaaS that turns a paid signup into a dedicated, per-user AI agent. Multi-tenant provisioning on Railway, hardened multi-provider supervisor (Claude, Gemini, OpenAI, xAI), credit-based billing, prompt-injection defense.

[Read the case study](./case-studies/hubclaw.md)

Tech: Next.js 16, TypeScript, Railway GraphQL, Anthropic + Gemini + OpenAI + xAI, Clerk, Autumn/Stripe, Vercel.

### saige2.0: AI tutor with hierarchical long-term memory

Production AI tutor for college students. Course-aware retrieval over pgvector embeddings and full-text hybrid search, with conversation auto-compaction and fire-and-forget background workers. Contributing engineer on a live production codebase.

[Read the case study](./case-studies/saige2.md)

Tech: Next.js 16 monorepo, TypeScript, Supabase + pgvector, Google Gemini, QStash, Stripe.

## Other work

- [RustLabsCli](./other-work.md): a multi-agent terminal UI in TypeScript and Ink. Listed for completeness; provider integration is in progress.

## Services

- **Agent systems engineering**: provisioning, orchestration, supervisor hardening, multi-provider routing.
- **LLM and agent evaluation**: provider comparison, regression detection, RAG quality, prompt-injection testing.
- **AI automation**: background pipelines, embedding and index workers, async LLM call chains.
- **Edtech engineering**: memory-augmented tutors, content modeling, freemium and trial systems.
- **Production AI plumbing**: Stripe webhooks, RLS-protected APIs, secret management, deploy pipelines.

## How I work

Open to short engagements, weekly retainer, or scoped project work. Comfortable joining an existing codebase, picking up the in-flight bugs, and shipping forward-compatible fixes without rewriting history.

## Contact

- Email: bzarnitz23@gmail.com
- GitHub: [Beandon13](https://github.com/Beandon13)

If you want me to evaluate, harden, or build a piece of your AI stack, send the system you want me to look at. I will come back with specifics, not a sales pitch.

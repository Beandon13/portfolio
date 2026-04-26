# HubClaw: Personal AI agent platform with a hardened supervisor

## One-line summary

A subscription SaaS that provisions private AI agents for each user and routes requests through a hardened multi-model supervisor.

## What it does

HubClaw turns a paid signup into a live, dedicated AI agent. Each user gets their own runtime spun up on Railway, with a personalized environment, model selection, and a credit-metered chat surface for issuing tasks. A supervisor model (Claude Haiku, with Gemini, OpenAI, and xAI fallbacks) sits in front of the agent and translates conversational requests into structured task directives.

## The problem

General-purpose AI chat is easy. Operating per-user agents with isolated state, billing, safety controls, and reliable provisioning is not. HubClaw solves the operational layer: how to stand up an agent on demand, scope it to one user, charge for usage, and prevent users from talking the supervisor into doing things it shouldn't.

## My role

Sole author. I designed and built the platform end to end: auth flow, billing integration, Railway provisioning logic, multi-provider supervisor, prompt-injection defense, dashboard UI, and deployment pipeline. About 158 of 161 commits on the repo.

## Technical scope

- **Frontend**: Next.js 16 (App Router), React 19, TypeScript, Tailwind v4, Framer Motion, React Three Fiber.
- **Backend**: Next.js API routes, server-side context fetching, streaming LLM responses.
- **Auth**: Clerk with cookie-based setup persistence across OAuth redirects.
- **Billing**: Autumn (feature-based credits) on top of Stripe.
- **Agent infrastructure**: Railway GraphQL API for service creation, env injection, volume attachment, and domain provisioning.
- **LLM providers**: Anthropic, Google Gemini, OpenAI, xAI; OpenRouter for the agent runtime.
- **Analytics**: PostHog. **Deployment**: Vercel.

## Key engineering work

- **Atomic, idempotent agent provisioning.** A subscription event spins up a Railway service with per-user env vars. The flow uses a provisioning lock to prevent double-provision races, rolls back orphaned services on partial failure, and caps blast radius at 100 services per project.
- **Prompt-injection defense for the supervisor.** User input passes a guard that detects more than twenty common injection patterns. Context is sanitized before it reaches the system prompt, and AI-generated task names are validated and bracket-escaped before reaching the agent.
- **Multi-provider supervisor with graceful fallback.** A single endpoint routes between Anthropic, Gemini, OpenAI, and xAI based on availability. If a provider key is missing or a request times out (30s cap), the next provider takes over without breaking the user-facing chat.
- **Credit-gated chat with fail-open billing.** Each message checks credits before processing and reports usage after. If the billing provider is unavailable, the system fails open rather than locking users out, and reconciles afterward.
- **Server-authoritative agent state.** Clients can never forge agent context. Every supervisor call fetches state server-side before assembling the prompt, so a malicious client can't pretend to be in a state it isn't in.
- **Dev/prod separation without code drift.** A `dev-user` shortcut bypasses auth and billing locally so the full stack runs without credentials, using the same code paths rather than special branches in the hot path.

## Challenges and constraints

The hardest part was treating the supervisor as untrusted infrastructure. A user can write anything in chat, and the supervisor has to translate that into directives an agent will obey. That is a textbook indirect-prompt-injection surface. Getting it right meant defense in depth: input sanitization, server-side context, output validation, directive escaping, and provider-side fallback when one model gets jailbroken in a way another doesn't.

On the infra side, Railway provisioning is asynchronous and partially failure-prone, so the flow had to be both idempotent (retries don't double-charge or double-provision) and self-cleaning (failed runs don't leave orphaned services consuming credits).

## Outcomes

- Shipped a full SaaS stack from auth through billing through agent runtime, deployed on Vercel with active Railway and Autumn integrations.
- Built a prompt-injection defense covering more than twenty attack patterns, with sanitized server-side context assembly and directive validation.
- Integrated four LLM providers behind one supervisor endpoint with timeout-bounded fallback.
- Wrote a provisioning system with locks, rollback, and a hard cap, designed to survive partial failures without wedging a user account.
- About 14,800 lines of TypeScript across 89 files and 20+ API routes.

## Why it matters for contract work

Most "agent" demos run a single LLM call inside a notebook. Operating real agents per user, on real infrastructure, charging real money, while resisting real attack surface is a different category of work. This is the system shape small AI startups need to ship to actual customers. The parts that look easy (provisioning, billing, supervisor hardening) are exactly where projects stall in production.

## Public-safe summary

HubClaw is a production-style SaaS that turns a paid signup into a dedicated, per-user AI agent. The supervisor routes between four LLM providers with timeout-bounded fallback and is hardened against prompt injection through input sanitization, server-side context assembly, and directive escaping. Agents are provisioned on Railway with atomic rollback, idempotent provisioning locks, and a hard service cap. Built on Next.js, Clerk, Autumn/Stripe, PostHog, and Vercel.

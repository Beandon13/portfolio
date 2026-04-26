# Other work

## RustLabsCli

A multi-agent terminal UI built with TypeScript and Ink. Sole author.

Phase 1 ships the full keyboard-driven dashboard: tabbed view (Run, Logs, Marketplace, Settings, Swarm), a 2x2 / 3x3 / 1x3 agent grid for spawning multiple independent agents in one terminal, file-attachment syntax (`@filename`), session persistence with conversation context per agent, and a config layer for OAuth or API-key auth.

Provider integration is the next phase. The orchestration layer is in place; live provider calls (OpenClaw and direct routes to Anthropic, OpenAI, Gemini, Groq) are mocked in this release. Listed here for completeness, not as featured production work. The Phase 2 release will move it into the featured section once real provider calls and the auth flow are wired.

Stack: TypeScript, Node 18+, Ink (React for the terminal), Commander, axios, zod, Conf for persisted config. Published to npm. CI/CD via GitHub Actions with npm OIDC trusted publisher.

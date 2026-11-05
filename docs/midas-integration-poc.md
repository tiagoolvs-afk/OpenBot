# MIDAS AI — OpenBot proof of concept

Status: approved, isolated discovery/PoC branch. This document does not enable production integration.

## Goal
Evaluate OpenBot as the agent orchestration and governed tool-execution layer for MIDAS AI, while MIDAS remains the product identity and system of record.

## Current repository facts
- OpenBot is a self-hosted template, not a hosted product/package.
- The stack includes Bun, Docker, PostgreSQL and CopilotKit Intelligence (managed or self-hosted).
- Agents communicate through AG-UI; tool calls pass through OpenBot's policy/audit gateway.
- The built-in `agent-bot` accepts an OpenAI-compatible Chat Completions endpoint through `OPENAI_BASE_URL`. The framework Bot has a narrower provider list.
- The `agent-llamaindex` name denotes the LlamaIndex framework, not a requirement to use a Llama model.

## Proposed PoC boundary
1. Run OpenBot separately from MIDAS and only in a development/test environment.
2. Keep MIDAS authentication, tenant identity and authorization authoritative.
3. Expose a minimal read-only MIDAS API adapter first; no write operations in the first test.
4. Pass only an authenticated, server-validated tenant/user context to the adapter. Never trust tenant IDs supplied by the browser or model.
5. Test one operational question against synthetic or explicitly approved test data.
6. Record tool decisions and outcomes; deny by default when identity, tenant, role or scope cannot be verified.
7. Keep the model provider configurable. Do not select or change the production model as part of this PoC.

## First acceptance tests
- OpenBot can start in an isolated environment with documented dependencies.
- A MIDAS test user can ask a question and receive an AG-UI response.
- The adapter returns only data belonging to that user's tenant.
- A different tenant's data is inaccessible, including through prompt injection or guessed identifiers.
- Tool calls are read-only and denied when authorization context is absent.
- No production MIDAS data, secrets, database, deployment or user interface is changed.

## Explicit non-goals
- No production deployment or connection to production databases.
- No changes to MIDAS production model/provider settings.
- No OS creation/editing/deletion, approvals, payments, messaging or other mutations.
- No cross-tenant search.
- No assumption that OpenBot replaces MIDAS authentication, database or business rules.

## Next engineering step
Map the MIDAS authentication/session and tenant-resolution flow to a small server-side, read-only adapter. Review the exact route and model implementations before writing the adapter. Then add automated tenant-isolation and authorization tests before connecting a model.

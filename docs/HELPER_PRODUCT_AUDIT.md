# HELPER — Product foundation audit

Status: initial repository inspection
Repository: jeanerickj10/helper
Base branch: develop
Working branch: helper/product-foundation

## Confirmed baseline

- The repository README identifies the application as Chatwoot and describes an omnichannel customer-support platform.
- The checked package manifest reports version 4.19.1.
- Backend: Ruby 3.4.4 and Ruby on Rails 8.0.5.2.
- Frontend dependencies include Vue 3 and Vite.
- Local development compose stack includes Rails, Sidekiq, Vite, PostgreSQL 16 with pgvector, Redis, and MailHog.
- The Gemfile includes Stripe, AI/LLM-related libraries (including ruby-openai, ruby_llm, ai-agents, pgvector, and Firecrawl), plus integrations for several messaging channels.
- The application configuration explicitly loads code, views, and initializers from the enterprise/ directory.
- The root LICENSE distinguishes MIT-licensed content from enterprise/ and third-party components. enterprise/LICENSE imposes separate production licensing requirements.

## Product direction

Build HELPER as an independent, multi-tenant SaaS customer communication and support platform. Preserve proven core functionality where licensing permits; add HELPER-owned onboarding, tenant administration, plan enforcement, billing experience, brand identity, and differentiated AI workflows.

## Critical audit items before production

1. Map every runtime dependency on enterprise/ and classify each feature against its applicable license.
2. Inventory tenant boundaries across accounts, users, inboxes, contacts, conversations, attachments, reports, background jobs, and API access.
3. Verify account isolation with automated cross-tenant authorization tests. Do not assume account scoping alone is sufficient.
4. Trace existing Stripe code and determine whether subscription lifecycle, webhook verification, plan entitlements, metering, invoices, failed-payment handling, and cancellation are complete.
5. Audit authentication, MFA/encryption configuration, secrets, file storage, backups, logging, rate limits, and incident response.
6. Identify branding, product-name, domain, email, and documentation references that still expose Chatwoot.
7. Review third-party licenses and provider terms for each enabled integration and AI capability.

## Recommended implementation sequence

### Phase 0 — Baseline and safety
- Keep develop untouched.
- Work on helper/product-foundation.
- Record current commit, install/build/test instructions, and known test status.
- Complete the enterprise and multi-tenancy audit before removing or replacing code.

### Phase 1 — SaaS foundation
- HELPER product configuration and brand layer.
- Signup and workspace onboarding.
- Workspace lifecycle and owner/admin/member roles.
- Tenant-safe authorization and regression tests.
- Super-admin operations and support tooling.

### Phase 2 — Commercial foundation
- Plans and feature entitlements.
- Stripe checkout and customer portal.
- Verified webhook processing and idempotency.
- Subscription states, grace periods, cancellation, and usage limits.
- Billing and usage pages.

### Phase 3 — Customer value
- Inbox, assignment, teams, tickets/workflows, knowledge base.
- HELPER AI grounded in each tenant's approved knowledge sources.
- Human handoff, usage quotas, auditability, and safe failure behavior.

### Phase 4 — Launch readiness
- Security and privacy review.
- Data export/deletion and retention policies.
- Monitoring, backups, restore drills, and support procedures.
- Pilot onboarding and product analytics.

## First release acceptance criteria

- A business can create its own workspace without operator intervention.
- A workspace owner can invite and manage agents.
- Users cannot access another workspace's data through UI, API, jobs, files, or reports.
- A workspace can select a plan and see its entitlements.
- Billing events are verified, idempotent, and reflected correctly in account status.
- The customer-facing product no longer presents itself as Chatwoot, subject to applicable attribution and license notices.
- A pilot business can connect at least one supported channel and manage real customer conversations.

## Explicit non-goals for the first release

- Rewriting the entire application.
- Building every industry-specific workflow.
- Releasing autonomous AI that can take consequential customer actions without human review.
- Enabling enterprise-only code without the required rights.
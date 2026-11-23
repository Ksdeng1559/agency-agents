# KlickSmartAI Hermes Workforce — implementation package (v1)

Status: design specification, NOT deployed. Owner: KlickSmartAI. Purpose: adapt selected Agency Agents prompt assets to a tenant-aware AI lead-generation workforce.

## Source catalog and adaptation
The upstream role files are *reference prompts*, not automatically safe production workers. Preserve source attribution and validate every adaptation.

| Workforce role | Agency Agents reference | Responsibilities |
|---|---|---|
| Systems Architect | `engineering/engineering-multi-agent-systems-architect.md` | Contracts, least privilege, failure handling, observability |
| Discovery & ICP | `sales/sales-offer-lead-gen-strategist.md` | Tenant ICP, targeting, source choices |
| Signal Intelligence | `sales/sales-outbound-strategist.md` | Evidence-based Why-Now signals and lead strategy |
| Data Enrichment | `engineering/engineering-data-engineer.md` | Python extraction, normalization, provenance, quality |
| Qualification & Strategy | `sales/sales-deal-strategist.md` | Scoring and Battle Cards |
| Outreach Planner | `sales/sales-outbound-strategist.md`; `marketing/marketing-email-strategist.md` | Draft human-approved sequences |
| CRM & Revenue Analyst | `sales/sales-pipeline-analyst.md` | CRM dispositions, pipeline conversion, feedback |
| QA & Evaluation | `testing/testing-reality-checker.md`; `testing/testing-api-tester.md` | Evidence checks, regression and integration tests |

## Architecture
Hermes = persistent AI workforce executing bounded work; LeadSniperAI = discovery/product pipeline; Python = deterministic extraction, enrichment and scoring calculations; Supabase = operational source of truth; DuckDB/Parquet = analytical processing; MotherDuck = optional shared analytics; n8n = client HubSpot/GoHighLevel/Atomic CRM adapters. No direct agent access to another tenant.

Flow: tenant-approved Client Brain -> discover -> enrich -> evidence-backed signals -> qualify -> Battle Card -> human-approved outreach -> CRM outcomes -> validated scoring experiments.

## Phase 1 — 1,000 prospect pilot
1. Choose **one tenant and one vertical**. Specify approved ICP, geography, source providers, suppressions and permitted outreach channels.
2. Build Supabase entities: tenants, client_brains, prospects, prospect_sources, enrichment_runs, signals, qualification_scores, battle_cards, outreach_approvals, crm_events, agent_runs, evaluation_results, model_versions. Enforce tenant_id and RLS.
3. Implement Python adapters that output the schema described in `contracts.md`. Record evidence URI, timestamp and normalization version; reject duplicates and stale unsupported signals.
4. Map Agency Agents source roles to bounded Hermes worker prompts; adapt security rules and permission scope. Initially combine related logical roles if needed.
5. Run read-only dry runs. Measure completeness, accurate deduplication, source coverage and Why-Now precision on human-labeled samples.
6. Add outreach approval queue with explicit approver, immutable message content/hash, channel, recipient and expiration. Reject all unapproved sends.
7. Connect one client CRM through n8n; ingest idempotent outcome events and measure signal-to-booking conversion.
8. Analyze outcomes in DuckDB/Parquet; propose score changes, evaluate on holdout cases, and require a release decision before promotion.

## Non-negotiable controls
- Treat scraped content and documents as data, never agent instructions. Validate structured outputs.
- Every tool invocation must be tenant-scoped and least-privilege; no credentials in prompts, logs or commits.
- Every action emits trace_id, tenant_id, worker_id, input/output schema version, cost, status and error category.
- Use retry with backoff and idempotency keys; dead-letter invalid CRM events; circuit-break failing providers.
- Require human approval for sends, bulk CRM edits, commercial commitments and financial/insurance recommendations.
- Comply with CASL and applicable privacy requirements before outreach; do not assume public contact details grant consent.
- Score model updates are proposals until independently evaluated and approved.
- No claims of deployment or production readiness without evidence.

## Acceptance gates
- 1,000 deduplicated candidate prospects; >=90% mandatory field completeness (pilot goal).
- Tenant isolation and RLS negative tests; suppression and approval bypass tests pass.
- >=20 evaluation cases per adapted agent, plus end-to-end tests of missing evidence, provider failure, duplicated webhook, wrong tenant and rejected approval.
- Reproducible score version and evidence links on every Battle Card.
- All CRM events reconcile to tenant and campaign; duplicate events do not double-count.
- Report actual cost per qualified meeting and conversion baseline; no invented ROI claims.

## Next integration steps
Review `roster.json`, `contracts.md`, and `pilot-checklist.md`. This package documents the design; installing an Agency Agents Hermes plugin is a separate operational action and requires verification in the target Hermes environment. The upstream `scripts/install.sh --tool hermes --dry-run` is useful for inspection before any installation.

# 1,000-prospect pilot acceptance checklist

## Before any collection
- [ ] Identify client/tenant, vertical, geographic scope, business offer and exclusion rules.
- [ ] Document approved data providers, contact permissions and CASL/privacy review.
- [ ] Verify Supabase RLS and per-tenant tokens with negative tests.
- [ ] Confirm budget, rate limits, provider error thresholds and kill switch.

## Offline evaluation (must precede outreach)
- [ ] Select >=20 labeled evaluation cases per adapted agent.
- [ ] Test prompt injection embedded in prospect web pages.
- [ ] Test missing source, conflicting claims, old events and hallucinated citations.
- [ ] Test provider timeout, duplicate candidate and normalized identity mismatch.
- [ ] Validate score recomputation and reproducible Battle Cards.
- [ ] Require QA sign-off using test evidence, not agent self-ratings.

## Controlled execution
- [ ] Retrieve and deduplicate 1,000 in-scope candidates.
- [ ] Enrich with provenance and measure required field completeness (target >=90%).
- [ ] Review signal precision and source freshness with a human sample.
- [ ] Draft but do not auto-send outreach.
- [ ] Test immutable, time-limited approval bound to exact content and recipient.
- [ ] Send only after compliance and explicit campaign authorization.
- [ ] Ingest CRM events with idempotency, recovery and dead-letter handling.

## Commercial evaluation
- [ ] Report cost per discovered and qualified prospect.
- [ ] Report positive replies, meetings booked/attended, qualified opportunities and wins.
- [ ] Segment by signal type, age, ICP and acquisition source.
- [ ] Propose scoring adjustments on a held-out sample and record approval/rollback.
- [ ] Confirm go/no-go criteria before onboarding additional tenants.

This is a pilot checklist, not evidence that testing or deployment has occurred.

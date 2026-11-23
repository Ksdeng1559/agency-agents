# Agent contract specification — v1 (proposed)

All objects are tenant-scoped. IDs are opaque strings. All UTC timestamps are ISO 8601. Schemas here are requirements for future implementation, not already deployed APIs.

## Shared envelope
```json
{"schema_version":"1.0","tenant_id":"tenant_001","trace_id":"trace_001","run_id":"run_001","agent_id":"intelligence","created_at":"2026-11-23T00:00:00Z","status":"proposed","payload":{}}
```
Reject missing tenant_id, trace_id, schema_version or unexpected enum values. Protect PII; log only necessary metadata.

## Prospect
```json
{"prospect_id":"p_001","tenant_id":"tenant_001","business_name":"Example Company","domain":"example.com","geography":"Vancouver, BC","source_url":"https://example.com","source_observed_at":"2026-11-20T00:00:00Z","dedupe_key":"example.com","suppressed":false}
```

## Why-Now signal
```json
{"signal_id":"s_001","prospect_id":"p_001","type":"hiring","claim":"Company posted for an IT manager","evidence_url":"https://example.com/careers","event_date":"2026-11-20","observed_at":"2026-11-21T00:00:00Z","window_days":90,"confidence":0.82,"inference":false}
```
If event_date is unknown, use null and downgrade freshness confidence. Store multiple evidence records where applicable. Unsupported signals are ineligible for outreach personalization.

## Qualification
```json
{"prospect_id":"p_001","score_version":"pilot-v1","fit":80,"urgency":65,"evidence_quality":90,"total":77,"decision":"review","rationale_signal_ids":["s_001"]}
```
Each component 0–100; total is computed deterministically by a separately versioned formula, not an unverified LLM assertion.

## Outreach approval
```json
{"approval_id":"a_001","tenant_id":"tenant_001","prospect_id":"p_001","channel":"email","content_sha256":"64-hex-character-content-hash","status":"pending","approved_by":null,"approved_at":null,"expires_at":"2026-12-01T00:00:00Z"}
```
Sender must verify recipient, consent/legal basis where required, message hash, tenant, suppression list, unexpired approval and exact content before dispatch. Idempotency key = tenant + campaign + prospect + message step.

## CRM outcome
```json
{"event_id":"crm_event_001","tenant_id":"tenant_001","prospect_id":"p_001","provider":"hubspot","provider_event_id":"external-123","campaign_id":"camp_001","outcome":"meeting_booked","occurred_at":"2026-11-23T00:00:00Z","received_at":"2026-11-23T00:00:01Z"}
```
Unique (tenant_id, provider, provider_event_id). Allowed outcome states should be centralized and versioned. Quarantine unmatched and cross-tenant records.

## Learning change
```json
{"experiment_id":"exp_001","score_version_from":"pilot-v1","score_version_to":"pilot-v2-candidate","evaluation_dataset":"holdout-001","baseline_precision":0.6,"candidate_precision":0.64,"status":"awaiting_human_approval"}
```
Never automatically promote a model from client CRM feedback without data-quality checks, regression testing and review.

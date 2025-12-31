# Example: Epic Breakdown

Shows how Tech Lead breaks down an Idea Issue into Epic + Stories.

## Original Idea Issue (from PO)

**Title:** Service health monitoring needed

**Body:**
> Our ops team can't tell if the service is running. We need some way 
> to check service health without looking at logs.
>
> Should be simple - just tell us if it's up.

---

## Tech Lead's Clarifying Questions

```markdown
@PO - Questions before creating Epic:

1. **Health definition:** What makes service "healthy"?
   - Just responds? 
   - Database connected?
   - All dependencies up?

2. **Response format:** Need specific data?
   - Just "ok/not ok"?
   - Include timestamp?
   - Include version?

3. **Consumers:** Who will call this?
   - Kubernetes probes?
   - External monitoring (Datadog, etc)?
   - Human debugging?

4. **Scope:** Include metrics (request count, uptime)?

Cannot proceed until clarified.
```

---

## PO Response

```markdown
@tech-lead - Answers:

1. **Health = service responds + timestamp**
   - No DB check needed yet (future)
   - No dependency checks

2. **Format:** `{"status": "ok", "timestamp": "2024-01-15T12:00:00Z"}`
   - Yes timestamp
   - No version needed

3. **Consumers:** 
   - Kubernetes liveness probe
   - Human debugging

4. **Scope:** Yes include basic metrics
   - Request count
   - Uptime seconds

Keep it simple!
```

---

## Created Epic

**Title:** EPIC: Service Monitoring Baseline v0.1.0

**Labels:** epic

**Body:**
```markdown
## Problem Statement
Operations team cannot determine service health without checking logs.
Need lightweight health and metrics endpoints for monitoring.

## Success Criteria
- [ ] Health endpoint for liveness probes
- [ ] Metrics endpoint for basic observability
- [ ] Tests for all endpoints
- [ ] Documentation updated

## Non-Goals (from PO clarification)
- Database connectivity checks
- Dependency health checks
- Version in response
- Advanced metrics (histograms, etc)

## Stories
- [ ] Story 1: Health endpoint (#2)
- [ ] Story 2: Metrics endpoint (#3)

## Architecture Notes
- **Stack:** Flask (existing)
- **Endpoints:** GET /health, GET /metrics
- **Test strategy:** pytest with test client fixture
- **Dependencies:** None new

## PO Clarifications
- Q: What makes healthy? A: Just responds + timestamp
- Q: Include metrics? A: Yes, basic (count + uptime)
- Documented in: Issue #1 comments
```

---

## Created Stories

### Story #2: Health Endpoint

```markdown
Parent: #1

## Success Criteria
1. GET /health returns 200 status code
2. Response JSON: {"status": "ok", "timestamp": "<ISO8601>"}
3. Timestamp in ISO8601 format with Z suffix
4. Unit tests for status code, response shape, timestamp format

## Test Plan
- test_health_returns_200
- test_health_response_has_status_ok
- test_health_response_has_timestamp
- test_health_timestamp_is_iso8601

## Non-Goals
- No authentication
- No detailed diagnostics
- No database checks

## DoR Checklist
- [x] Success criteria measurable
- [x] Test plan defined
- [x] Non-goals stated
- [x] Architecture in Epic
```

---

### Story #3: Metrics Endpoint

```markdown
Parent: #1

## Success Criteria
1. GET /metrics returns 200 status code
2. Response includes request_count (integer)
3. Response includes uptime_seconds (integer)
4. Counter increments on each request
5. Unit tests for all above

## Test Plan
- test_metrics_returns_200
- test_metrics_has_request_count
- test_metrics_has_uptime
- test_metrics_count_increments

## Non-Goals
- No Prometheus format
- No histogram data
- No per-endpoint breakdown

## DoR Checklist
- [x] Success criteria measurable
- [x] Test plan defined
- [x] Non-goals stated
- [x] Depends on: none (parallel with #2)
```

---

## Key Takeaways

1. **Question first** - Tech Lead asked 4 clarifying questions
2. **Document answers** - PO responses captured in Epic
3. **Measurable criteria** - Each criterion has a test
4. **Clear non-goals** - Prevent scope creep
5. **Linked artifacts** - Stories reference Epic

---

**See also:** [02-pr-with-evidence.md](02-pr-with-evidence.md) for how Story #2 became a PR

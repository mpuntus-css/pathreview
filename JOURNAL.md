## Week 7 — Issue selection

**Issue link:** [https://github.com/ascherj/pathreview/issues/155#issuecomment-4976060218]

**Issue title:** [Health check references settings.redis_host, which does not exist on Settings #155]

**Tier:** [X] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
[The /health endpoint in api/routes/health.py fails during runtime because it attempts to access a non-existent redis_host attribute on the Settings model. When a GET /health request is received, the handler immediately raises an AttributeError instead of completing its status check. This occurs because core/config.py does not declare redis_host within the application configuration schema. Resolving this issue requires defining the redis_host field in the Settings model so the health probe can properly inspect and report Redis connection health without crashing.]

**Branch name:** [fix/155-health-endpoint-attribute-error]

**Setup confirmation:** [X] App runs locally at localhost:5173

**Cohort ledger:** [X] Issue added to cohort ledger


## Week 8 — Reproduction & solution planning

**Reproduction summary:**
[Triggered a GET /health request and observed an AttributeError caused by a missing redis_host attribute on the Settings object. Verified via grep redis_host core/config.py that the configuration model lacks this field definition.]

**PLAN.md link:** [https://github.com/mpuntus-css/pathreview/blob/main/PLAN.md]


## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
[Updated api/routes/health.py to use settings.redis_url for the Redis health probe instead of non-existent attributes (redis_host and redis_port). All sub-tasks related to route updates and refactoring have been completed.]

**Next steps:**
[Add regression unit tests for GET /health to verify from_url instantiation and ensure pytest tests/unit passes completely.]

**Blockers:**
[]

---

### Check-in 2 (end of week)

**PR link:** [https://github.com/mpuntus-css/pathreview/pull/1]

**Branch:** [fix/155-health-endpoint-attribute-error]

**What you built:**
[Refactored the Redis health probe in api/routes/health.py to utilize settings.redis_url instead of accessing non-existent redis_host and redis_port attributes on the Settings model. This eliminates the runtime AttributeError and aligns the endpoint directly with the existing configuration schema in core/config.py.]

**Tests added or updated:**
[tests/unit/test_health.py — Added a unit test to verify that GET /health successfully invokes redis.Redis.from_url(...) and returns a healthy response status when Redis answers the ping() check.]

**Self-review confirmation:** [X] make check passes  [ ] make test-unit passes

**Draft PR feedback received from:** [none]

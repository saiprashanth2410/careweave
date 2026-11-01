# QA report: CareWeave API (functional) 2026-11-01
Surface: server API on local JSON store (Atlas/Firebase/Gemini/Exasol disabled). Browser UI not tested.
Static: server tsc clean, 34 -> 41 tests pass (incl. WhatsApp WIP), client tsc + 12 vitest pass.
Verified OK: 401 without/with bad token, 403 role boundaries, login rate limit (429 after 10), CORS rejects unknown origin, attachment path traversal 404.
## ISSUE-001 (high, verified) async handler throw kills server
POST /api/patients {"name":123,...} -> TypeError, process exit. Fix: asyncRoutes.ts wrapper. Regression: src/asyncRoutes.test.ts. Commit 8ca8e6b.
## ISSUE-002 (medium, verified) malformed JSON body returned 500 -> now 400 (413 for too large).
## ISSUE-003 (medium, verified) POST /api/patients accepted garbage/future dob -> now 400.
## Deferred (low)
- phone number format not validated; patientId is random CW-5digits, collisions possible; no tests for doctor safety routes (HIGH_COLLISION override) at API level; browser UI not covered.

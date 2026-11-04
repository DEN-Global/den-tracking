# DEN Order-scoped authenticated location validation

Private service package, version 0.1.0, JSON contract version `1.0.0`. Plain ESM with no third-party runtime dependencies. Backend-compatible exports from `src/index.js`: `recordLocation, latestLocation`.

Run `npm test` and `npm run check` on Node 24. CI runs both. Tests cover successful state transitions and authorization/idempotency failure paths; they do not establish production readiness.

## Integration boundary

Derive actor/assignment from DB, check authorization before selecting rows. Persist unique point ID and increasing sample sequence atomically. Consumers only see their active assignment. Timestamp validation assumes server milliseconds. Expire location retention under an approved privacy policy.

State transitions return new state; callers must store results transactionally with compare-and-swap/unique constraints. In-memory arrays are command fixtures, not production storage. The parent backend vendors a pinned copy plus checksum manifest for private-repository builds; this package is the authoritative source. Client-provided actors, statuses, receipt flags and balances have no authority.

## Current readiness

Validation, participant isolation, stale/unavailable/simulated labels work locally. No map provider, live GPS capture, route computation, ETA model, background location consent or retention job.

No external resources are provisioned, no credentials are present, and no live money is moved. See `contract.json` for the version envelope; domain payload validation is exported by the functions above.

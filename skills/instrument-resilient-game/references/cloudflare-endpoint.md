# Cloudflare telemetry endpoint

This is one provider adapter, not the telemetry domain contract. Use a small
Cloudflare Worker or Pages Function as a replaceable ingestion boundary when
Cloudflare fits the project. D1 is suitable when incidents need relational
queries and durable rows; Analytics Engine may suit aggregate metrics. Verify
current product availability, limits, and pricing before choosing.

If the Cloudflare account or local credentials are missing, use the beginner
setup workflow in `architect-web-first-indie-game` when available. Guide the
human through the account and scoped-token steps, prepare an ignored local
`.env`, and have them enter credentials privately. Verify access without
printing secret values. Use their explicit choice of an alternative provider
when present.

Current first-party references:

- D1 overview: <https://developers.cloudflare.com/d1/>
- D1 Worker binding: <https://developers.cloudflare.com/d1/get-started/>
- Workers rate limiting: <https://developers.cloudflare.com/workers/runtime-apis/bindings/rate-limit/>
- CORS handling: <https://developers.cloudflare.com/workers/examples/cors-header-proxy/>
- Worker lifecycle and `waitUntil()`:
  <https://developers.cloudflare.com/workers/runtime-apis/context/>

## Endpoint contract

- Accept only `POST` and the required `OPTIONS` preflight.
- Restrict allowed origins to actual game origins where feasible; set `Vary:
  Origin` when responses vary by origin.
- Require JSON content type and enforce a small body limit before parsing.
- Validate the complete versioned schema and reject unknown variants.
- Use prepared statements with bound values for D1 writes.
- Apply abuse controls before expensive parsing or database work. Rate limiting
  is permissive rather than exact accounting, so do not use it as the only
  integrity mechanism.
- Return a minimal response. Never echo rejected payloads or internal errors.
- Record endpoint failures in provider observability without logging private
  request bodies.

Await writes whose success defines ingestion. If ancillary work may continue
after the response, register it with `ctx.waitUntil()`; a floating promise may
be canceled when the invocation ends. Use a Queue rather than `waitUntil()` for
work that needs reliable delivery or longer processing.

## Data operations

Define retention and deletion before public collection starts. Keep schema
migrations in source control. Maintain queries that answer actual debugging
questions by build and signature. Test malformed requests, duplicate event IDs,
rate limiting, D1 failure, CORS preflight, and endpoint unavailability.

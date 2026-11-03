# ADR 0002: AI and external integration boundaries

## Context

[Roadmap #1](https://github.com/BrandonTing/coffee_atlas/issues/1) requires trusted AI invocation, provenance, and explicit confirmation before persistence. [Foundation #2](https://github.com/BrandonTing/coffee_atlas/issues/2) defers authentication and billing choices. External responses are untrusted, including future SQL results.

## Decision

- Expose AI operations through a small injected Effect service. Add only operations needed by the current feature slice; tests supply a controlled implementation.
- Start with a direct provider SDK inside the implementation. Defer TanStack AI until a feature benefits from its provider adapters, conversation controls, or tool handling; this does not select a provider or authentication mechanism.
- Keep provider calls, credentials, and provider-specific response formats in server-only implementation code.
- Validate data entering from every external source with Effect Schema before application logic treats it as trusted typed data. This includes client requests, AI responses, future SQL results, and other integrations.
- Each integration owns decoding its external representation and translating expected failures into the Effect typed error channel. Return validated application data or a typed failure.
- The first AI interaction uses a complete response, validated before display. Streaming is not required for this slice.
- Schema validity does not establish factual truth. Preserve provenance, unknowns, and the roadmap's distinction between AI hypotheses and user observations.

## Alternatives

- Return raw provider responses: simpler adapter, but callers duplicate decoding and depend on provider formats.
- Create a generic integration/AI framework: anticipates future capabilities, but introduces abstractions without concrete feature requirements.
- Adopt TanStack AI immediately: supplies provider adapters and Svelte chat controls, and supports complete structured responses without streaming. The first slice currently needs little of this plumbing; reassess when it removes concrete work. See its [Svelte API](https://tanstack.com/ai/latest/docs/api/ai-svelte) and [structured output documentation](https://tanstack.com/ai/latest/docs/structured-outputs/overview).

## Consequences

Workflows depend on application contracts rather than provider SDKs or database drivers. Integration tests must exercise malformed external payloads as well as successful decoding; a fake service alone cannot verify an adapter's validation.

Open: the first concrete service operation, external schema, failure variants, and provider implementation. Authentication and billing remain deferred. Do not introduce a generic integration framework to enforce this rule.

## Status

Proposed — reflects interview agreements; awaiting maintainer review of the complete ADR. No application code implemented.

# ADR 0001: Application architecture

## Context

[Foundation #2](https://github.com/BrandonTing/coffee_atlas/issues/2) supports the discovery and review-before-save workflows in [roadmap #1](https://github.com/BrandonTing/coffee_atlas/issues/1). This decision session is documentation-only, in Deep Learning mode.

## Decision

- Use SvelteKit 3.1.1 and Effect 4.1.2 as the initial foundation baseline. Deploy the full-stack application to Cloudflare Workers with Static Assets using `@sveltejs/adapter-cloudflare` 8.0.2: Workers run server rendering and trusted handlers; Static Assets serve browser JavaScript, CSS, and images.
- Svelte owns presentation, temporary editing state, and ordinary interaction handlers; those handlers may use plain TypeScript.
- SvelteKit server handlers stay thin: decode requests, invoke Effect workflows, and translate results into safe browser responses.
- Use Effect for integration workflows, schema validation, typed expected failures, and dependency injection. Keep business rules testable independently of HTTP and components.
- Choose state machines when a feature benefits from explicit states and transitions, regardless of time pressure. When introducing one, integrate XState with `@xstate/effect`, subject to compatibility verification. No state machine is required merely for an AI request.
- Test the first slice deterministically with an injected fake AI Provider. Cover valid output, malformed external data, and provider failure. Add a browser test through the real SvelteKit boundary for submission and visible result/recovery behavior. Live-provider checks are separate and optional.

## Alternatives

- Put business rules in route handlers: fewer initial boundaries, but tests become coupled to SvelteKit.
- Use Effect throughout UI handlers: a uniform programming model with additional integration work that current requirements do not justify.
- Mock browser responses alone: simpler setup, but bypasses server workflows and validation.

## Consequences

Application workflows can be tested without a browser or production credentials. Server handlers must deliberately translate Effect results into browser-safe responses.

Open: install/build/runtime verification; feature-specific transport; StyleX evaluation when styling the application. TanStack AI is deferred in [ADR 0002](0002-ai-integration-boundary.md). State-machine adoption remains feature-specific. No persistence technology or conversation recovery mechanism is selected here; those choices belong to the slices that need them.

Version evidence checked on 2026-11-03 (baseline approved in interview; not yet installed):

- SvelteKit 3.1.1 is released. Its [tagged package manifest](https://github.com/sveltejs/kit/blob/%40sveltejs/kit%403.1.1/packages/kit/package.json) requires Svelte `^5.57.1`, Vite `^8.0.12`, the Svelte Vite plugin `^7.0.0`, and TypeScript `^6.0.0` when used. The published npm metadata requires Node `>=22.17`.
- Effect 4.1.2 is released, verified against npm metadata and [official releases](https://github.com/Effect-TS/effect/releases). This is not a beta-only dependency.
- The [XState integration documentation](https://github.com/statelyai/xstate/blob/next/packages/xstate-effect/README.md) describes experimental `@xstate/effect` targeting XState v6 alpha and Effect 4. Published npm metadata reports `@xstate/effect` `1.0.0-alpha.12`, with peers Effect `^4.0.0` and XState `^6.0.0-alpha.73`; the XState alpha tag reports `6.0.0-alpha.73`. These facts do not prove Svelte lifecycle compatibility; verify a concrete machine's snapshot subscription, cancellation, cleanup, and failure handling before adopting it for a feature.

SvelteKit provides [server-only import enforcement](https://svelte.dev/docs/kit/server-only-modules). Keep credentials and integration implementations in server-only modules; returned browser data must also be deliberately limited.

Cloudflare compatibility: the [official SvelteKit adapter](https://svelte.dev/docs/kit/adapter-cloudflare) supports Workers and Pages, including server features. The [adapter 8.0.2 manifest](https://github.com/sveltejs/kit/blob/main/packages/adapter-cloudflare/package.json) declares Kit `^3.0.0-next.0` (which includes 3.1.1) and Wrangler `^4.118.0`. Cloudflare also provides a [SvelteKit Workers deployment guide](https://developers.cloudflare.com/workers/framework-guides/web-apps/sveltekit/).

Node `>=22.17` is a build/tooling requirement; production executes in the Workers runtime. Use Web APIs and Workers-compatible SDKs; [Node compatibility](https://developers.cloudflare.com/workers/runtime-apis/nodejs/) does not provide an unrestricted Node server. During foundation implementation, build and run the actual Effect/adapter boundary in local Workers emulation; later verify the selected AI SDK there. Documentation and matching peer ranges are not an executed deployment test. Cloudflare deployment does not select a database or Workers AI as the model provider.

Operational recommendations pending implementation review: read secrets from server-side Cloudflare bindings, supply integration dependencies within request handling, and keep user-specific state out of globals. Bound request work to the Workers lifecycle; state-machine use does not imply a persistent background actor.

## Status

Proposed — reflects interview agreements; awaiting maintainer review of the complete ADR. No application code implemented.

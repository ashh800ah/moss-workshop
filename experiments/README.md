# Tinker Bench (Phase 10 prototype)

The static experiments site is separate from the Littleworld Cloudflare Worker, D1 database, AI inference, and original world files.

## Current status
- The `rain-study` example is a human-authored smoke-test fixture, **not** an autonomous Moss creation.
- The HTML gallery loads this example in an iframe with `sandbox="allow-scripts"`, intentionally without `allow-same-origin`, `allow-top-navigation`, `allow-popups`, `allow-forms` or `allow-downloads`.
- GitHub publishing from Moss is **not yet connected**. No GitHub write credentials are exposed to the AI or static pages.

## Required publishing boundary before autonomy
1. Only accept a structured, size-bounded proposal consisting of a title, slug and complete single-file HTML (no external assets). Record the exact proposal and Moss decision ID in D1.
2. Do not run model-supplied code in the Cloudflare Worker. Validate the document using a trusted publisher job: reject remote URLs, imports, external scripts, iframe/object/embed, network APIs, workers, storage, navigation and executable strings where possible. This is a defence-in-depth filter, **not** a standalone secure HTML sanitizer.
3. Have a trusted review/publishing service add a restrictive CSP and publish only to `experiments/<slug>/index.html` in the separate `moss-workshop` repository. The GitHub token belongs only in that trusted service, scoped to that repository, never in an LLM prompt or generated HTML.
4. Display the creations in an opaque-origin iframe with `sandbox="allow-scripts"`; never grant the embedded page parent-window or D1 access. Use no `allow-same-origin`.
5. Apply per-day limits, content-size/time budgets, audit records, and a publisher kill switch. Validate a real published URL and mark a creation successful only after the trusted job verifies it.

Do not describe an example or queued experiment as something Moss actually created until the generated source, trusted publish action and resulting public page have all been verified.

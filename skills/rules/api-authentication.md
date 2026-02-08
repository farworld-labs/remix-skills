# API Authentication Rules

- Use existing user API keys for this pass.
- Prefer `Authorization: Bearer sk_live_*`.
- Do not place keys in browser-side code.
- Rotate keys immediately if leaked.
- Include `Idempotency-Key` on submit mutations.

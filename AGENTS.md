# AGENTS.md

- English only. Compact. No secrets, tokens, person names, or host addresses.
- Scope: issues and tests for `device-xiaomi-laurel` only.
- Shared kernel changes belong in `omnipha/linux-sm61x5`. Package changes belong in `ports`. Build definitions belong in `images`.
- Never commit firmware blobs, credentials, or a full kernel tree.
- Link every cross-repo issue and pull request. Do not duplicate work.
- Device supervisor registration is separate. Nothing in this repo adds it to the main-host allowlist or auto-claims its issues.

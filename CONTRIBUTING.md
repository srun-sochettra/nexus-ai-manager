# Contributing

Keep changes focused on a reproducible issue or a concrete user benefit. Read the [README](README.md), [architecture notes](docs/architecture.md) and [security guidance](SECURITY.md) first.

## Development

Follow the root setup using a compatible external business API and synthetic records. The external backend is not included here; do not invent its schema, permissions, seeded accounts or deployment procedure.

Preserve the distinction between Google/NextAuth sessions and external API tokens. A visible USER or ADMIN badge is not proof that a backend request is authorised.

The current scripts are:

```powershell
npm run lint
npm run build
```

There is no test script in the supplied manifest. Add a focused regression check for changed non-trivial behaviour rather than claiming existing coverage. Report exact commands and results, including any missing backend, credentials or runtime dependencies.

## Review priorities

- Trace record changes from the form through the API client to the actual backend contract.
- Test failed reads/writes, duplicate submissions, expired sessions and denied actions without losing user input.
- For AI changes, verify request validation, authorised data scope, response shape and failure handling. A successful JSON parse does not establish correctness.
- For image handling, test URL restrictions, redirects, content type, size and timeouts before enabling untrusted input.
- For UI changes, check keyboard operation, focus, mobile layouts, long content and loading/error/empty states. Do not claim accessibility conformance from screenshots.
- Keep credentials, real employee data and provider responses out of commits and public diagnostics.

A mocked provider test is not a live-provider check. A build is not a successful deployment. Separate these verification levels in change reports.

## Pull requests

Explain the problem, focused change, checks performed and remaining gaps. Include data/security implications, rollback and backend compatibility where material. Keep unrelated branding, dependency and architecture changes separate.

Use [SECURITY.md](SECURITY.md) for sensitive reports. This guide does not establish a licence or maintainer response deadline.

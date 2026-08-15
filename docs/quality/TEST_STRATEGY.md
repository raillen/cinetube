# Test Strategy

## Pirâmide

- Domain unit: recommendation, progress, list rules, IDs, conflict decisions.
- Application: use cases com fake ports.
- Adapter contract: TMDB/SuperFlix/storage/backup com fixtures.
- DB integration: migrations, FK, FTS, transactions, backup restore.
- Frontend component: keyboard/state/rendering.
- E2E: first-run, search->details->play, continue watching, profile switch, backup/restore.

## Provider tests

Nunca depender somente de chamadas live em CI. Fixtures versionadas + smoke live opcional com secrets protegidos.

## Performance

Benchmarks reprodutíveis para startup, Home data assembly, Search local, recommendation, DB migrations e image-cache indexing.

## Security

Tests para injection, path traversal no backup, malicious archive/payload, binding exposure, secret redaction e corrupted DB/backup.

## Release gate

Build, testes críticos, migrations, security review, accessibility smoke e performance regression check precisam de evidência.

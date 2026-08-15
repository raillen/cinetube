# Repository Structure

```text
cmd/desktop/                 Wails entrypoint
internal/domain/             entidades/regras puras
internal/application/        use cases
internal/ports/              interfaces externas
internal/adapters/
  superflix/
  tmdb/
  storage/
  filesystem/
internal/platform/           OS-specific secure storage/player
transport/wails/             bindings/DTO mapping
frontend/src/
  components/
  features/
  api/MediaClient.ts
  api/WailsMediaClient.ts
  routes/
database/migrations/
docs/
.ai/
.atlas/
tests/fixtures/
```

## Regras

- `internal/domain` não depende de adapter.
- `transport/wails` não contém business rules.
- `frontend/components` não importa bindings gerados diretamente.
- Provider packages não importam UI.
- SQL canonical fica versionado em migrations/queries; DB runtime não vai ao Git.

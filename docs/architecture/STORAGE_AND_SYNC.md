# Storage and Sync Architecture

## v1: local first

O banco local é a fonte de verdade para perfis e atividade. Cloud não participa do caminho crítico.

### Durável local
Perfis, settings pessoais, progress, events/history, lists, recommendation weights e backup metadata.

### Compartilhado/derivado
Metadata cache, provider availability e image cache. Pode ser reconstruído.

## Driver

A direção preferida é avaliar `tursogo` via `database/sql` por compatibilidade com banco local, ausência de CGO e caminho futuro de sync. Essa escolha só é bloqueada após validar estabilidade, criptografia, migrations e desempenho em Windows/Linux/macOS. Um `StoragePort` permite fallback a outro SQLite-compatible driver sem afetar domínio.

## Futuro cloud

Turso Cloud é opcional. Para sync verdadeiro, preferir isolamento por tenant/account. Backup automático pode usar storage pertencente ao usuário (ex.: Google Drive) e não exige replicar o DB em nossa cloud.

## Outbox

Se sync for ativado futuramente, mutações locais geram registros em `sync_outbox` na mesma transação. Push/pull é batch e nunca bloqueia ação local.

## Conflitos

- listas/favorites: updated_at + tombstone.
- watch events: append-only.
- progress: sessão/updated_at, não simplesmente MAX(position).
- settings: last-write-wins por campo quando aceitável.

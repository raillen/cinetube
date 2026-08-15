# Domain Model

## Identidade

`MediaID` é interno e estável; `ExternalIDs` guarda TMDB/IMDb quando existentes. Entidades locais duráveis usam UUIDv7 quando a biblioteca escolhida for madura e testada; persistir em 16 bytes onde trouxer benefício sem prejudicar portabilidade.

## Entidades principais

- `Media`: id, type, títulos, datas, runtime, rating summary.
- `ExternalIDs`: TMDB, IMDb e futuros IDs por provider.
- `MediaDetails`: synopsis, genres, keywords, credits, images, videos, seasons.
- `Person`: identidade e créditos.
- `Profile`: identidade local, nome, avatar, policy/PIN metadata.
- `WatchProgress`: media/episode, position, duration, session, timestamp, completed.
- `WatchEvent`: started/progress/completed/rewatched/abandoned.
- `UserList` / `UserListItem`: favorite/watch-later/custom.
- `TasteWeight`: feature/value/weight/version.
- `ProviderDescriptor`: capabilities, health, priority.
- `PlaybackTarget`: destino opaco resolvido pelo adapter.

## Regras

- `ProfileID` está presente em todo estado pessoal.
- Metadata não é duplicada por perfil.
- Provider URLs não são chaves primárias de mídia.
- Remoções sincronizáveis futuras usam tombstone/updated_at, não hard delete imediato quando necessário.

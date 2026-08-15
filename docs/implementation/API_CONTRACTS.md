# Application API Contracts

## MediaClient

Operações mínimas: `GetHome`, `Search`, `GetDetails`, `GetPerson`, `GetLibrary`, `SetFavorite`, `SetWatchLater`, `SetProgress`, `CreateList`, `UpdateList`, `Play`, `GetProfiles`, `SelectProfile`, `GetSettings`, `UpdateSettings`, `ExportBackup`, `ImportBackup`, `GetProviderStatus`.

## Query models

`SearchQuery`: text, types, yearMin/yearMax, countries, languages, genres, keywords, people, crew, ratingMin/max, runtimeMin/max, sort, availableOnly, cursor/page.

## Error model

Erros públicos normalizados: validation, not_found, provider_unavailable, provider_rate_limited, playback_unavailable, storage, permission, backup_invalid, backup_incompatible, cancelled, internal.

Nunca retornar stack trace, token ou provider secret para UI.

## Cancellation

Search, metadata e provider HTTP devem aceitar cancellation. A UI cancela query antiga ao trocar filtro/texto.

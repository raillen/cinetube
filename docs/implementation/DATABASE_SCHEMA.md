# Database Schema

## Regras gerais

- FK habilitadas.
- transactions em mudanças compostas.
- queries parametrizadas.
- migrations forward e estratégia de rollback/restore.
- `created_at`/`updated_at` UTC onde necessário.
- UUIDv7 para entidades locais duráveis quando aprovado; IDs externos nunca substituem ID interno.

## Shared tables

`media`, `external_ids`, `genres`, `media_genres`, `keywords`, `media_keywords`, `people`, `media_people`, `provider_availability`, `metadata_cache`, `image_cache_index`.

## Profiles

`profiles(id, name, avatar_ref, pin_hash, child_policy, created_at, last_used_at)`.

## Personal state

`watch_progress(profile_id, media_id, episode_key, position_ms, duration_ms, play_session_id, completed, updated_at)`.

`watch_events(id, profile_id, media_id, episode_key, event_type, position_ms, occurred_at)`.

`user_lists(id, profile_id, type, name, created_at, updated_at)`; types reservados: favorite/watch_later/custom.

`user_list_items(id, list_id, media_id, position, created_at, updated_at, deleted_at)`.

`hidden_media(profile_id, media_id, reason, updated_at)`.

`taste_weights(profile_id, feature_type, feature_id, weight, algorithm_version, updated_at)`.

`profile_settings(profile_id, key, value)`.

## Search

FTS5 para títulos/títulos alternativos e, quando útil, pessoas/keywords cacheadas. Índices comuns: external ID, profile+updated_at, media+provider, list+position.

## Future sync

Reservar `device_id`, `updated_at`, tombstones e `sync_outbox` apenas quando a feature for implementada; evitar pagar complexidade de sync no v1 além do necessário para portabilidade.

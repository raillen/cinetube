# ADR-0002 — Providers separados por capability

- Status: **Accepted**
- Date: 2026-08-15

## Decision

Separar Catalog, Metadata, Playback, Images e Trailers em ports independentes.

## Rationale

SuperFlix e TMDB cobrem capacidades diferentes e qualquer provider pode desaparecer.

## Consequences

Mais interfaces pequenas; melhora fallback/testes.

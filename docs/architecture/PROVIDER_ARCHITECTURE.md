# Provider Architecture

## Objetivo

Trocar uma fonte sem reescrever UI, domínio, biblioteca ou recommendation engine.

## Ports por capability

```go
type CatalogProvider interface { Browse(context.Context, CatalogQuery) ([]MediaRef, error) }
type MetadataProvider interface { Search(context.Context, SearchQuery) ([]Media, error); Details(context.Context, MediaRef) (MediaDetails, error) }
type PlaybackProvider interface { Resolve(context.Context, PlayRequest) (PlaybackTarget, error) }
type ImageProvider interface { Images(context.Context, MediaRef) ([]ImageRef, error) }
type TrailerProvider interface { Trailers(context.Context, MediaRef) ([]Trailer, error) }
```

Um provider pode implementar apenas algumas capabilities.

## Registry

Mantém prioridade, health, capability matrix, backoff e fallback. Fallback só ocorre entre adapters compatíveis e não deve alterar `MediaID`.

## Adapters iniciais

- SuperFlix: catálogo/disponibilidade e playback documentado.
- TMDB: metadata, search/discover, pessoas, imagens e trailers.

## Segurança

- Base URLs centralizadas/configuráveis.
- Timeout, cancellation, retry limitado e rate awareness.
- Não confiar em HTML/JS retornado por provider.
- Player remoto sem privileged bindings.
- Não implementar scraping/extraction de stream não documentada.

## Testes

Cada adapter tem fixtures/contract tests e pode ser substituído por mock. Provider failure é estado esperado, não panic.

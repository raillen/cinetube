# Frontend / Backend Boundary

## Regra

Componentes Svelte dependem de `MediaClient`, nunca de bindings Wails diretamente.

```ts
export interface MediaClient {
  home(): Promise<HomeData>
  search(query: SearchQuery): Promise<SearchResult>
  details(id: string): Promise<MediaDetails>
  play(request: PlayRequest): Promise<void>
  setFavorite(id: string, value: boolean): Promise<void>
}
```

`WailsMediaClient` implementa o contrato chamando DTOs gerados. Futuro `WebMediaClient` chama backend web ou web-core.

## Backend

Packages `domain` e `application` não importam Wails. O adapter Wails só converte tipos, context/cancellation e erros.

## DTOs

- IDs como strings nas fronteiras JS/Go.
- Datas em ISO-8601 UTC.
- Evitar `any` e payloads gigantes.
- Detalhes completos só na tela Details; Home usa `MediaSummary`.

## Eventos

Usar eventos somente para mudanças realmente assíncronas (playback state, provider health, import progress), não como substituto para chamadas simples.

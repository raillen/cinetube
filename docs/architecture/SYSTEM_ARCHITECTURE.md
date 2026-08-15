# System Architecture

## Visão

```text
Svelte UI
  -> MediaClient
    -> WailsMediaClient (transport)
      -> Go Application Services
        -> Domain
          -> Ports
             -> Metadata adapter (TMDB)
             -> Catalog/Availability adapter (SuperFlix inicial)
             -> Playback adapter (SuperFlix inicial)
             -> Storage adapter (SQLite/Turso local)
             -> Filesystem image cache
             -> Backup adapter
```

## Camadas

### Presentation
Svelte 5, Tailwind v4 e Lucide. Mantém estado visual, navegação e acessibilidade; não conhece SQL, tokens de provider ou filesystem.

### Transport
Bindings Wails v3 traduzem DTOs e chamadas assíncronas. Não contém regra de negócio.

### Application
Casos de uso: `GetHome`, `Search`, `GetDetails`, `SetProgress`, `ToggleFavorite`, `ExportBackup`, `Play`.

### Domain
Tipos e regras estáveis: Media, Profile, Progress, List, RecommendationSignal, ProviderCapability. Sem dependências de framework.

### Ports/Adapters
Acesso externo ocorre por interfaces pequenas por capability. Erros são normalizados.

## Invariantes

- Toda dependência aponta para dentro do domínio, não o contrário.
- UI e provider nunca compartilham modelos diretamente.
- Playback remoto é trust boundary.
- Cache é derivado; banco pessoal é durável.
- Cloud é adapter opcional, não camada obrigatória.

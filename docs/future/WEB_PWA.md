# Future Web/PWA

**Pós-v1 desktop.**

## Arquitetura proposta

Shared Svelte UI/contracts -> `WebMediaClient` -> TypeScript/Hono API ou web-core -> SQLite WASM/OPFS. PWA adiciona manifest/service worker/cache shell.

## Hosting

Cloudflare Workers + static assets é direção preferida por simplicidade e edge runtime; revalidar limites/preços quando a implementação começar.

## Backend

Hono + TypeScript; validação de schema; auth somente para premium/cloud se existir. Não exigir conta para estado local PWA.

## Local storage

SQLite WASM em Worker com OPFS quando suporte/browser permitir. Solicitar persistent storage e oferecer backup de arquivo porque browser storage não é equivalente a filesystem desktop.

## Provider constraints

CORS e credenciais podem exigir backend proxy autorizado para metadata/API, mas não proxy de mídia destinado a contornar restrições de provider.

## Auto-backup

Pode autorizar Google Drive/appDataFolder do próprio usuário e armazenar backup cifrado, potencialmente sem banco cloud CineTube para o caso backup-only.

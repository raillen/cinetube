# Scope and Roadmap

## P00 — Foundation

- Wails v3 + Go + Svelte/Vite/Tailwind/Lucide.
- Domain/application/ports/adapters.
- DB local e migrations.
- contratos de provider, storage, backup e frontend.
- baseline de performance e threat model.

## P01 — Desktop Core

- Perfis locais.
- Home contextual.
- Metadata/provider adapters.
- Search/Explore/Details.
- Player presenter desktop.

## P02 — Personal Library

- Progresso/histórico.
- Favoritos/watch later/listas.
- recomendação local.
- cache e imagens.
- backup/restore.

## P03 — Desktop 1.0 Hardening

- performance budget gates.
- segurança e acessibilidade.
- fallback de providers.
- instaladores/release multi-desktop.
- documentação de usuário/operação.

## Pós-v1

### Web/PWA
Frontend compartilhado, `WebMediaClient`, Hono/TypeScript, Cloudflare, SQLite WASM/OPFS, service worker e backup local/Web opcional.

### Android
Wails v3 mobile quando adequado, presenter de player mobile, Android Keystore e backup Google Drive/appDataFolder opcional.

### Cloud premium
Turso Cloud ou storage do usuário para backup/sync opcional; entitlement/billing separado do perfil local; Mercado Pago somente server-side.

## Regra de escopo

Nenhum item pós-v1 pode bloquear ou degradar o desktop local-first. Preparar contratos é permitido; implementar infraestrutura cloud antecipadamente não é.

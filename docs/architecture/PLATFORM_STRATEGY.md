# Platform Strategy

## Desktop v1

Wails v3 + Go backend + Svelte frontend. Windows/Linux/macOS são alvos; matriz final de release depende de CI e validação do WebView/driver local.

## Web/PWA pós-v1

Reutiliza UI e contratos, não os bindings Go. `WebMediaClient` pode falar com Hono/TypeScript e usar SQLite WASM/OPFS para estado local. Cloudflare é opção de hosting/backend.

## Android pós-v1

Reutilizar Wails v3 mobile quando tecnicamente adequado e manter `PlayerPresenter`, `SecureStorage`, `BackupProvider` específicos da plataforma.

## Regra

Nenhuma plataforma futura força abstração artificial na v1. Criar interfaces onde existe boundary real; evitar “framework universal” antecipado.

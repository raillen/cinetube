# Threat Model

## Assets

Dados de perfis, histórico/progresso, listas, encryption keys, provider credentials, backup files e integridade do aplicativo.

## Trust boundaries

1. Svelte <-> Wails bindings.
2. Go <-> local DB/filesystem.
3. Go <-> TMDB/SuperFlix/other providers.
4. Main app <-> remote player/web content.
5. Futuro client <-> cloud API/Turso/Drive/Mercado Pago.

## Principais ameaças

- remote player chamar binding privilegiado;
- token/secret exposto ao frontend/log;
- SQL injection ou migration corrupta;
- backup malicioso/path traversal/zip bomb;
- provider response oversized/malformed;
- XSS/remote navigation em WebView;
- DB/key copiados juntos;
- supply-chain dependency;
- future multi-tenant authorization failure;
- forged payment webhook.

## Mitigações

Least privilege, allowlists de bindings/navigation, schema validation, size/time limits, parameterized SQL, secure storage, authenticated encryption, signed release, dependency scanning, server-side billing verification e isolamento tenant quando cloud existir.

## Security gate

Qualquer mudança em player, storage encryption, backup parser, auth, sync ou billing requer security-reviewer e testes adversariais.

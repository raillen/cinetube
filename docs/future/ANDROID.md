# Future Android

**Pós-v1 desktop.**

## Base

Reutilizar frontend Svelte, contracts e backend Go via Wails v3 mobile quando validado. Abstrações específicas: `PlayerPresenter`, `SecureStorage`, `BackupProvider` e lifecycle/background work.

## Dados

Mesmo schema lógico local e perfis. Encryption key em Android Keystore. Cache adaptado a quota/storage móvel.

## Backup

- arquivo portátil continua universal;
- opção premium/conveniência: Google OAuth + `drive.appdata` para backup cifrado no Drive do usuário;
- Android Auto Backup pode ser camada extra, não substituir backup explícito por app.

## Performance

Perfis de imagem mais agressivos, bateria e background sync controlados, evitar sockets/realtime desnecessários.

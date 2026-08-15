# Data Security

## Local DB

Preferir encryption-at-rest se driver estável e suportado. A master key é aleatória e fica em secure storage do OS; DB e key nunca no mesmo diretório em plaintext.

## Profile PIN

Hash com KDF apropriada + salt; serve apenas como controle local de acesso ao perfil. Não usar PIN de 4–6 dígitos como DB key.

## Backup

AEAD + Argon2id/approved KDF para password-protected export. Header é autenticado. Restore é staging/transacional.

## Logs

Redact tokens, cookies, auth headers, full provider URLs com query secrets, backup keys e payloads pessoais. Telemetria remota não é requisito e deve ser opt-in se algum dia existir.

## Database best practices

UUIDs, FK/UNIQUE/CHECK, prepared/parameterized queries, migrations versionadas, menor privilege, testes de corrupção/rollback e vacuum/index maintenance conforme evidência.

## Secrets

Development secrets via env/OS secret store. Production secrets nunca incorporados no JS bundle.

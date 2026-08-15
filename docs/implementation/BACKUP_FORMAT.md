# Portable Backup Format

## Objetivo

Backup deve ser independente do arquivo físico do DB e funcionar entre versões/plataformas.

## Extensão

`.cinetube-backup` (nome final pode mudar antes de 1.0 sem quebrar o schema interno).

## Conteúdo lógico

Manifest versionado + profiles + history/events + progress + lists + hidden + profile settings + recommendation weights. Excluir posters, backdrops, metadata cache, FTS derived indexes e provider availability.

## Segurança

Backup sem senha pode ser oferecido com aviso claro; modo recomendado usa:

1. random Data Encryption Key;
2. senha -> Argon2id -> Key Encryption Key;
3. wrap/encrypt da DEK;
4. payload compactado cifrado com AEAD;
5. header autenticado com format version/KDF params/salt.

Usar bibliotecas auditadas. Nunca implementar primitive criptográfica própria.

## Restore

Validar magic/version, auth tag, schema compatibility, IDs/constraints e espaço. Restaurar em transação/staging; manter backup anterior até sucesso.

## Portabilidade

Cache deve ser reconstruído após restore. Nenhum provider credential deve entrar no backup por padrão.

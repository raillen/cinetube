# Security Policy

## Princípios

CineTube adota least privilege, local-first, secrets-out-of-client, validação em fronteiras e isolamento de conteúdo remoto.

## Reporte

Não publique vulnerabilidades com detalhes exploráveis em issue pública. Use um canal privado de segurança do mantenedor quando disponibilizado.

## Dados sensíveis

- Chaves de DB/backup devem ser geradas aleatoriamente e guardadas em secure storage do SO.
- PIN de perfil não é chave de criptografia.
- Credenciais de providers/cloud/billing nunca são commitadas.
- Backups com senha usam KDF resistente (Argon2id ou sucessor aprovado) + AEAD via biblioteca consolidada.
- Logs não podem conter tokens, URLs autenticadas, cookies, chaves ou payloads pessoais completos.

## Conteúdo remoto

Player/provider remoto é conteúdo não confiável. Ele não recebe bindings privilegiados do aplicativo. Qualquer exceção exige threat model e ADR.

## Dependências

Atualizações de Wails, WebView, Svelte, Go, driver SQLite/Turso e bibliotecas criptográficas devem ser acompanhadas de revisão de segurança e testes de regressão.

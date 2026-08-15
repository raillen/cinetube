# Future Cloud & Billing Security

**Pós-v1; não implementar antecipadamente.**

## Identidades separadas

`LocalProfile != BackupIdentity != Subscription/Entitlement`. Uso local não exige conta.

## Auto-backup

Pode usar storage do usuário (Google Drive appDataFolder) para reduzir exposição/custo. Arquivo é cifrado localmente antes do upload. Scope deve ser mínimo.

## Turso Cloud

Para sync, preferir isolamento por tenant/account e tokens de menor privilégio/curta duração. Credencial administrativa nunca vai para client. Authorization é aplicada por backend/tenant boundary, não confiando apenas em IDs enviados pelo cliente.

## Web backend

Hono/TypeScript em Cloudflare é direção proposta; validar payloads, secure headers, CSRF quando cookie-based, rate limits e auth. Segredos em server environment.

## Mercado Pago

- access token somente backend;
- `external_reference` aponta para opaque entitlement/account ID, não perfil local;
- status premium muda após webhook validado e reconciliação server-side;
- verificar assinatura/autenticidade do webhook conforme documentação vigente;
- idempotency para criação/handling de eventos;
- não armazenar dados de cartão.

## Princípio

Premium compra conveniência de auto-backup/sync; nunca bloqueia acesso aos próprios dados locais ou ao backup manual.

# Future Cloud / Premium

## Princípio comercial

Cobrar por conveniência de auto-backup/sync, não por acesso aos dados locais. Aplicativo funciona sem assinatura.

## Opção A — Auto-backup user-owned storage

Google Drive appDataFolder recebe `.cinetube-backup` cifrado. CineTube mantém apenas entitlement/billing mínimo. Reduz storage cost e exposição de dados.

## Opção B — Turso Sync

Para cross-device state sync real, Turso Cloud pode manter banco por tenant/account ou outra estratégia isolada. Revalidar quotas/custos e authorization model.

## Identidade

Conta local não existe. `BackupIdentity` online só nasce ao ativar serviço. Google OAuth pode servir como identidade; magic link/passkey são alternativas futuras. Não criar senha CineTube sem necessidade.

## Billing

Mercado Pago backend-only. Entitlement identificado por UUID opaco, webhook verificado, operações idempotentes e reconciliation. O client consulta entitlement; não decide sozinho que pagamento é válido.

## Cancelamento

Cancelar premium encerra auto-backup/sync futuro, mas não apaga nem bloqueia dados locais. Definir retention cloud transparente se nossa infraestrutura armazenar dados.

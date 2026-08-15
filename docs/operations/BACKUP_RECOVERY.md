# Backup and Recovery Runbook

## Export

Gerar snapshot lógico consistente, validar, compactar, cifrar se solicitado e escrever atomicamente (`temp -> fsync quando aplicável -> rename`).

## Import

Nunca sobrescrever DB ativo diretamente. Validar arquivo, decifrar, parsear schema, importar em staging, executar integrity checks e só então trocar/merge conforme política.

## Falha

Manter DB original intacto. Exibir erro categorizado sem conteúdo sensível.

## Recovery

Se DB local corromper: preservar cópia para diagnóstico, tentar restore do último backup explícito e reconstruir caches. Provider/cache nunca substituem dados pessoais.

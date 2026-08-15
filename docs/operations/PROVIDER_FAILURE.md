# Provider Failure Runbook

## Sintomas

Timeout, DNS/TLS, 4xx/5xx, rate limit, schema incompatível, player indisponível.

## Ações automáticas

- cancellation/timeout;
- retries limitados com backoff somente em erros transitórios/idempotentes;
- circuit/health degradation quando repetitivo;
- fallback por capability/prioridade;
- cache stale-while-revalidate apenas para metadata permitida.

## UX

Biblioteca local permanece navegável. Informar “fonte temporariamente indisponível” sem stack trace. Permitir retry manual e troca de provider em Settings.

## Incidente de contrato

Desabilitar adapter afetado via configuração/release sem quebrar domínio. Atualizar contract fixture e ADR se houver mudança estrutural.

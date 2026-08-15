# ADR-0004 — Avaliar tursogo como storage local preferido

- Status: **Accepted with release gate**
- Date: 2026-08-15

## Decision

Implementar StoragePort e avaliar tursogo/database/sql antes de congelar driver.

## Rationale

Local-first, sem CGO e futuro sync são alinhados ao produto; criptografia/estabilidade precisam prova.

## Consequences

Fallback para SQLite-compatible driver permanece possível.

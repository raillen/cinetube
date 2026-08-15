# Developer Getting Started

## Antes de codificar

Leia `ENTRYPOINT.md`, `PROJECT_STATE.md`, Goal ativo, `SYSTEM_ARCHITECTURE.md`, `STACK.md` e ADRs relevantes.

## Bootstrap esperado

Instalar Go/toolchain Wails v3, Node package manager fixado pelo projeto e dependências frontend. Criar `.env` a partir de template futuro sem secrets reais no Git.

## Ordem de implementação P00

1. Wails shell + Svelte shell.
2. `MediaClient` e Wails adapter.
3. domain/application packages.
4. provider/storage interfaces + mocks.
5. DB migration inicial multi-profile.
6. Home/Search/Details/Library shells.
7. tests/security/perf baseline.

## Regra

Não implementar SuperFlix/TMDB diretamente dentro de componentes só para “fazer funcionar rápido”. Primeiro contrato, depois adapter.

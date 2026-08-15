# Technology Stack

## Desktop v1

- Wails v3 — shell desktop/bridge.
- Go — application/domain, HTTP, storage, cache, backup, recommendation, provider registry.
- Svelte 5 + Vite — UI compilada e build frontend.
- Tailwind CSS v4 — design tokens/utilities sem runtime de framework.
- Lucide Svelte — ícones.
- `database/sql` — boundary de persistência.
- `tursogo` — candidato preferido a driver local, sujeito a release gate.
- TanStack Virtual — somente listas realmente longas.

## Evitar inicialmente

SvelteKit, ORM, state manager global, component library pesada, carousel library e client-side query cache duplicando o backend Go.

## Dev/test

Go test, staticcheck/golangci-lint conforme decisão de toolchain, Vitest, Testing Library, Playwright e benchmarks Go/perf harness.

## Dependência nova

Precisa justificar: ganho funcional, bundle/runtime cost, licença, atividade/manutenção, vulnerabilidades e alternativa nativa.

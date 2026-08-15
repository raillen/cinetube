# CineTube

CineTube é um media center local-first, leve e orientado a desempenho para descoberta, organização e reprodução de filmes, séries, animes e outros conteúdos disponibilizados por provedores externos.

O primeiro produto é um aplicativo desktop construído com **Wails v3 + Go + Svelte 5 + Vite + Tailwind CSS v4 + Lucide**, com persistência local baseada em SQLite/Turso local. Web/PWA e Android são extensões planejadas pós-v1.0 e devem reutilizar os contratos e a maior parte da interface.

## Princípios

- **Local-first:** perfis, histórico, progresso, favoritos, listas, preferências e recomendações pertencem primeiro ao dispositivo.
- **Sem conta obrigatória:** criar e usar perfis locais não exige cadastro, login ou cloud.
- **Baixo consumo:** memória, CPU, GPU, I/O, rede e tamanho de bundle são tratados como requisitos funcionais.
- **Providers substituíveis:** catálogo, metadados e reprodução são acessados por portas independentes; SuperFlix e TMDB são apenas adapters iniciais.
- **Privacidade por padrão:** recomendações são determinísticas e calculadas localmente, sem IA e sem perfil comportamental remoto.
- **Backup portátil:** exportação/importação por arquivo é parte essencial do produto. Cloud e auto-backup são opcionais.
- **Sem hospedagem de mídia:** CineTube não hospeda, armazena, distribui ou retransmite arquivos audiovisuais. A reprodução referencia provedores externos. Metadados e imagens promocionais podem ser cacheados para desempenho.

## Escopo v1.0 desktop

Home contextual finita, pesquisa avançada, exploração, detalhes completos, reprodução, múltiplos perfis locais, continuar assistindo, histórico, favoritos, assistir mais tarde, listas personalizadas, recomendações locais, cache controlado, gerenciamento de providers, backup/restore e hardening de segurança/performance.

## Fora de escopo

- comentários, notas ou comunidade pública;
- hospedagem ou redistribuição de arquivos audiovisuais;
- extração de streams não documentados de terceiros;
- conta cloud obrigatória;
- cloud sync, PWA e Android como condição de v1.0.

## Documentação

Comece em [`docs/ATLAS.md`](docs/ATLAS.md). Agentes devem começar por [`ENTRYPOINT.md`](ENTRYPOINT.md), `atlas.json`, `PROJECT_STATE.md` e o Goal ativo.

> Estado: documentação/arquitetura pronta para iniciar implementação. Nenhuma implementação funcional é declarada por estes documentos.

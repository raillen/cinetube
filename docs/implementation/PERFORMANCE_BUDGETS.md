# Performance Budgets

Os números abaixo são **targets iniciais**, não alegações atuais; devem ser calibrados após baseline em hardware de referência.

## Startup

- janela/shell visível: alvo <= 1.5 s em SSD de referência;
- Home útil com cache quente: alvo <= 2.0 s;
- nenhum provider remoto bloqueia criação da janela.

## Memória

- definir baseline de shell vazio primeiro;
- Home deve manter conjunto de posters limitado e cache de imagens fora do heap JS quando possível;
- qualquer regressão >15% de RSS em fluxo equivalente requer investigação.

## UI

- interação/scroll: meta 60 fps onde hardware suporta;
- tasks longas no main thread >50 ms devem ser diagnosticadas;
- Search keystroke não dispara request imediato repetitivo.

## Rede/imagens

- poster card ~200–300 px de largura conforme DPR/layout;
- detail poster ~400–500 px;
- backdrop ~780–1280 px em modo equilibrado;
- evitar imagem original se não necessária.

## Cache

Perfis sugeridos: econômico ~150 MB, equilibrado ~300 MB, alto ~1 GB; configuráveis e sujeitos a benchmark/UX.

## Gate

Benchmark registra hardware, OS, build, dataset, cold/warm cache e mediana/p95 quando aplicável.

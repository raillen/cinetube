# UI Performance Rules

1. Home finita; não virtualizar o que é pequeno.
2. Search/Explore/History/filmography podem usar TanStack Virtual quando medição justificar.
3. Posters pequenos: solicitar CDN size próxima do render target; não baixar original para card.
4. Detail poster médio; backdrop balanceado, evitando imagens desnecessariamente 4K.
5. `loading=lazy`/IntersectionObserver para imagens fora da viewport.
6. Cachear arquivo comprimido recebido; não transcoding runtime por padrão.
7. Usar `$state.raw` para grandes coleções substituídas como bloco e keyed each.
8. Evitar blur/backdrop-filter amplo, box-shadows caros e animações de layout contínuas.
9. Sem trailer/preview autoplay.
10. `content-visibility:auto` em seções longas quando compatível e medido.
11. Cancelar busca anterior quando query muda; debounce inicial ~300 ms.
12. Detalhes/cast/trailer podem carregar progressivamente depois do conteúdo principal.

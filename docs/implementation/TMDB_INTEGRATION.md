# TMDB Integration

## Papel inicial

MetadataProvider, Search/Discover, pessoas, imagens, vídeos/trailers e referências canônicas externas.

## Uso

- Home/Details usam metadata cacheada.
- Search avançada combina discover/search com availability local/provider.
- `append_to_response` ou agrupamento equivalente pode reduzir round-trips quando apropriado.
- Keywords representam subgêneros/temas mais granulares.
- Pessoas são filtradas por ID, não apenas nome textual.

## Imagens

Solicitar tamanho apropriado ao contexto. Cache local registra source, path, dimensions, bytes e last_access. Não usar original em cards.

## Trailers

Exibir sob ação explícita. Sem autoplay.

## Attribution/licensing

A interface/release deve cumprir requisitos vigentes de atribuição e termos da API. Não assumir que metadata/imagens podem ser redistribuídas sem condições.

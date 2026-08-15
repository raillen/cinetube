# User Flows

## Primeiro uso

`Abrir -> criar perfil local -> Home`. Nenhum cadastro externo é exigido.

## Continuar série

`Home -> Continuar assistindo -> série -> episódio corrente -> Play -> sair -> progresso persistido localmente`.

## Descobrir filme

`Search/Explore -> filtros -> resultados -> Details -> trailer opcional -> Play/Watch Later/Favorite`.

## Pesquisa por pessoa

`Search -> filtro Pessoa -> autocomplete remoto/local -> selecionar pessoa -> resultados disponíveis -> Details`.

## Trocar perfil

`Profile menu -> selecionar perfil -> invalidar state pessoal -> recarregar Home`. Metadata/cache compartilhados são reaproveitados.

## Backup manual

`Settings -> Backup -> Export -> senha opcional -> arquivo .cinetube-backup`. Restore valida versão, integridade e compatibilidade antes de alterar dados.

## Provider indisponível

`Play/Search -> adapter retorna erro classificado -> registry tenta fallback autorizado -> UI mostra estado recuperável sem perder biblioteca local`.

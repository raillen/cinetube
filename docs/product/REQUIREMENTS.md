# Product Requirements

## Funcionais — v1 desktop

### Perfis
- Criar, editar, excluir e trocar perfis locais.
- PIN opcional; perfil infantil é permitido como política local.
- Sem guest mode.
- Troca de perfil não reinicia o aplicativo.

### Home
- Home finita, sem scroll/feed infinito.
- Continuar assistindo com episódio/posição.
- Assistidos recentemente.
- Recomendações locais explicáveis.
- Novos episódios quando metadata/provider permitir.
- Máximo inicial sugerido: 4–6 blocos e 6–12 itens por bloco.

### Descoberta e busca
- Pesquisa por texto com debounce/cancelamento.
- Filtros: tipo, ano/faixa, país, idioma, gênero, keyword/subgênero, ator/pessoa, direção/equipe, nota, duração e disponibilidade.
- Explorar com grid virtualizado apenas quando necessário.

### Detalhes
- Poster, backdrop, sinopse, título original, ano/data, gêneros, países, idiomas, duração, notas por fonte, elenco/equipe, fotos, coleções, temporadas/episódios e trailer quando disponível.
- Trailer nunca inicia automaticamente.

### Biblioteca
- Favoritos.
- Assistir mais tarde.
- Continuar assistindo.
- Histórico.
- Listas personalizadas.
- Ocultos / não tenho interesse.

### Playback
- Resolver conteúdo pelo PlaybackProvider ativo.
- Isolar conteúdo remoto de capabilities privilegiadas.
- Permitir troca/fallback de provider futuramente.

### Backup
- Exportar e importar arquivo portátil versionado.
- Não incluir cache reconstruível.
- Permitir backup protegido por senha.

## Não funcionais

- Startup e UI devem permanecer responsivos em hardware modesto.
- Sem segredo em frontend.
- Offline deve permitir biblioteca local, histórico e configuração; playback/metadata remota podem ficar indisponíveis.
- Banco e migrations devem suportar recuperação e compatibilidade.
- Toda feature crítica deve ser navegável por teclado.
- Contratos de domínio não dependem de Wails, SuperFlix ou TMDB.

## Non-goals v1

PWA, Android, sync cloud, billing Mercado Pago, auto-backup Google Drive, comentários, ratings públicos, chat, social graph, download de mídia e servidor de streaming.

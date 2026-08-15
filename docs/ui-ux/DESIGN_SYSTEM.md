# Design System

## Direção

Visual escuro, moderno e cinematográfico sem excesso de efeitos. Hierarquia clara, foco em poster/backdrop e informação. Evitar glassmorphism pesado, blur de grandes áreas e animações custosas.

## Tokens

Definir via Tailwind v4 `@theme`: background, surface, surface-hover, text, muted, accent, positive, warning, danger; spacing em escala consistente; radius pequeno/médio; shadow sutil.

## Tipografia

Uma família sans legível; limitar pesos carregados. Títulos podem usar peso 600/700; corpo 400/500. Evitar fontes remotas obrigatórias: preferir system stack ou assets empacotados/licenciados.

## Componentes essenciais

AppShell, TopNav, ProfileMenu, MediaCard, HorizontalRail, MediaGrid, SearchBar, FilterPanel, DetailHero, CreditCard, EpisodeList, ProgressBar, PrimaryButton, IconButton, Dialog, Toast, EmptyState, Skeleton e ProviderStatus.

## Motion

Transições curtas e transform/opacity. `prefers-reduced-motion` obrigatório. Sem autoplay visual ou preview de vídeo.

## Ícones

Lucide Svelte com import nominal; não carregar pacote inteiro por runtime dinâmico.

# Product Vision

## Problema

Clientes de streaming e agregadores de mídia costumam priorizar volume visual, tracking e frameworks pesados. CineTube busca uma experiência de descoberta e reprodução rica, semelhante a um serviço de streaming moderno, mas com baixo consumo de hardware, dados pessoais locais e independência de uma única fonte de conteúdo.

## Proposta

Um aplicativo desktop rápido e enxuto que organiza metadata de múltiplas fontes, referencia playback externo, mantém biblioteca/progresso local e oferece pesquisa avançada e recomendações determinísticas sem IA.

## Pilares

1. **Performance percebida e medida.** UI aparece antes de dados não críticos; imagens são dimensionadas; listas longas virtualizadas somente quando necessário.
2. **Local-first.** Perfil e comportamento não dependem de servidor.
3. **Provider-agnostic.** Falha ou substituição de provider não destrói biblioteca nem histórico.
4. **Frontend/backend desacoplados.** A UI pode viver em Wails hoje e PWA no futuro.
5. **Privacidade.** Recomendações e preferências são processadas localmente.
6. **Simplicidade operacional.** Backup por arquivo funciona sem conta; cloud é opcional.

## Público

Usuários que desejam um cliente de mídia elegante e completo, inclusive em computadores modestos, com controle local de biblioteca e perfis.

## Métrica norteadora

CineTube deve permanecer responsivo em hardware limitado sem sacrificar os fluxos essenciais de descoberta, retomada e reprodução. Toda evolução que aumenta significativamente RAM/CPU/GPU/bundle precisa demonstrar valor medido.

# Multi-profile Architecture

## Princípio

Perfis são locais e não equivalem a contas cloud.

## Shared
`media`, `people`, metadata cache, image cache, provider availability/configuração global.

## Profile-scoped
progress, watch events/history, favorites, watch later, custom lists, hidden media, taste weights, search history e preferências pessoais.

## Active Profile
O backend mantém `ProfileContext` thread-safe. A UI chama `getFavorites()` em vez de repetir `profile_id` em toda chamada; o application service resolve o perfil ativo. Operações administrativas que afetam outro perfil exigem explicit ID/policy.

## PIN
Opcional, hashado com KDF apropriada. PIN não cifra o banco e não substitui secure storage.

## Child policy
Pode filtrar certificação/gêneros e restringir configurações sensíveis. Deve ser aplicada no backend, não apenas escondida pela UI.

## Sem guest mode
Guest/ephemeral profile é non-goal por decisão de produto.

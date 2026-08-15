# Contributing

## Change workflow

- Escolha/defina Goal ou issue.
- Carregue contexto mínimo.
- Escreva teste/contract quando aplicável.
- Faça mudança pequena.
- Rode gates relevantes.
- Atualize docs somente se comportamento estável mudou.

## Arquitetura

Nova dependência transversal, novo provider contract, mudança de storage/sync ou quebra de frontend/backend boundary exige ADR/review.

## Performance

UI changes com potencial de adicionar imagens/DOM/JS, cache ou background work precisam de medição. Não otimizar por folklore; não aceitar regressão evidente sem justificativa.

## Security

Nunca commitar secrets. Conteúdo/provider remoto é untrusted. Backup parser e migrations são security-sensitive.

## Scope

Não reintroduzir public comments/ratings, guest mode, cloud-required accounts, infinite Home ou media hosting sem nova decisão explícita/ADR.

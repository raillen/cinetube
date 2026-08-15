# Workforce Dependencies

## Objetivo

CineTube não versiona cópias locais completas dos Agents, Skills e Recipes do Project Atlas. O repositório mantém somente manifests de dependência e resolve o workforce necessário sob demanda.

Essa política aplica os princípios **least workforce**, **least context** e **pointer over payload**: um Goal deve materializar somente os recursos de IA necessários para a tarefa atual.

## Fonte canônica

A fonte de workforce está definida em `.ai/orchestration/workforce-sources.json`.

A fonte inicial é `raillen/project-atlas-framework`, fixada por **commit SHA**, nunca por `main` ou outra referência móvel. O commit pinado torna a resolução reprodutível: o mesmo projeto recebe as mesmas instruções de workforce até uma atualização explícita do pin.

Paths canônicos:

- Agents: `src/project_atlas/resources/workforce/agents/{id}`
- Skills: `src/project_atlas/resources/workforce/skills/{id}`
- Recipes: `src/project_atlas/resources/workforce/recipes/{id}`

Os manifests `.ai/agents/manifest.json`, `.ai/skills/manifest.json` e `.ai/recipes/manifest.json` são listas de dependências, não evidência de que o conteúdo já esteja materializado.

## Resolução por Goal

Cada Goal pode declarar `workforce.bundles` ou recursos individuais. O resolver deve:

1. Ler o Goal ativo.
2. Expandir os bundles em `.ai/orchestration/workforce-bundles.json`.
3. Remover duplicatas.
4. Verificar primeiro o cache global validado.
5. Verificar o cache do projeto.
6. Baixar somente os recursos ausentes da source pinada.
7. Validar que o commit remoto corresponde ao `expected_commit`.
8. Materializar os recursos somente em cache derivado.
9. Carregar no contexto apenas os recursos necessários ao passo atual.

Não é permitido baixar o catálogo inteiro apenas por conveniência.

## Cache

Ordem de resolução:

1. `~/.project-atlas/workforce/project-atlas-framework/{ref}` — cache global compartilhado.
2. `.atlas/cache/workforce/project-atlas-framework/{ref}` — cache derivado do projeto.
3. Source remota pinada.

`.atlas/cache/` não é fonte de verdade e não deve ser commitado.

Se a rede estiver indisponível, somente recursos já presentes em cache **e previamente verificados** podem ser usados. Se um recurso necessário não estiver disponível, a execução deve falhar de forma explícita em vez de substituir silenciosamente por conteúdo diferente.

## Integridade e supply chain

Regras obrigatórias:

- nunca resolver de `main`, `master`, `latest` ou tag móvel;
- o commit SHA da source deve estar versionado no CineTube;
- falha de integridade bloqueia o recurso;
- upgrades são explícitos e revisáveis;
- conteúdo remoto não verificado nunca recebe permissões de agente;
- secrets, credenciais e tokens não são armazenados nos manifests;
- o cache pode ser apagado a qualquer momento e reconstruído da source pinada.

O pin de commit é a garantia mínima de integridade. O resolver pode registrar adicionalmente digest por artifact durante materialização para acelerar verificações futuras.

## Bundles CineTube

Os bundles atuais são:

- `foundation-core` — exploração, arquitetura, implementação, revisão, testes e documentação;
- `ui-quality` — frontend, UX, design system e acessibilidade;
- `persistence` — banco, cache e benchmarks;
- `provider-integration` — integrações HTTP/provider e contratos;
- `security` — arquitetura/revisão de segurança e threat modeling;
- `release-quality` — quality gate e release verification;
- `issue-workflow` — criação e triagem de issues;
- `future-web` — reservado ao Web/PWA pós-v1.

Bundles são conveniência de seleção. Eles não devem ser carregados integralmente no contexto se o passo atual precisa de apenas uma parte.

## P00-G01

O primeiro Goal usa:

- `foundation-core`
- `ui-quality`
- `persistence`
- `provider-integration`
- `security`

Isso não significa carregar todos simultaneamente. O orquestrador seleciona o menor subconjunto compatível com cada Task do Plan DAG.

## Upgrade do Project Atlas

Atualizar workforce é uma mudança deliberada:

1. escolher um novo commit/tag imutável do Project Atlas;
2. atualizar `workforce-sources.json`;
3. comparar Agents/Skills/Recipes realmente usados pelo CineTube;
4. revisar mudanças de instrução, permissões e requisitos;
5. executar os testes/goals de conformance relevantes;
6. somente então aceitar o novo pin.

Nunca fazer auto-upgrade de workforce em background.

## Relação com roteamento de LLMs

Workforce e modelo são conceitos independentes:

- **Agent** define o papel.
- **Skill** define procedimento/conhecimento operacional.
- **Recipe** define fluxo reutilizável.
- **Model policy** escolhe qual LLM executa o papel.

A política de modelos fica em `.ai/orchestration/model-policy.json` e os fallbacks em `.ai/orchestration/fallbacks.json`. O mapeamento específico de modelos do CineTube é deliberadamente tratado separadamente para permitir otimização por qualidade, tokens e custo.

## Implementação do resolver

O comportamento acima é contrato do projeto mesmo antes de existir um comando automatizado.

Quando o Project Atlas oferecer um resolver nativo compatível, o CineTube deve consumi-lo. Até lá, ferramentas/agentes podem obter os paths exatos da source pinada via Git/GitHub e materializá-los no cache derivado, respeitando integralmente esta política.

Um futuro comando pode assumir a forma conceitual:

```text
atlas workforce resolve --goal P00-G01
```

Esse nome não deve ser tratado como CLI já disponível até existir implementação e teste no framework.

# Integração Hermes Agent — CineTube

## Objetivo

Executar o CineTube pelo Hermes Agent sem permitir que a conveniência de `provider: auto`, modelos default, auxiliary fallbacks ou delegation substitua a política de modelos do Project Atlas.

A identidade operacional de uma chamada é:

`provider lógico Atlas + provider runtime Hermes + model + effort`

A execução só é válida quando o runtime real corresponde ao par aprovado no Atlas.

## Bootstrap obrigatório

Hermes suporta `.hermes.md` como arquivo de contexto de projeto de maior prioridade. No CineTube, `.hermes.md` é apenas o bootstrap nativo e **sempre manda começar por `ENTRYPOINT.md`**.

Ordem:

```text
.hermes.md
  -> ENTRYPOINT.md
  -> AGENTS.md
  -> atlas.json
  -> PROJECT_STATE.md
  -> docs/ATLAS.md
  -> Goal ativo
  -> hermes-policy.json
  -> contexto mínimo do Goal/Task
```

Não ler toda a documentação nem todo o repositório por padrão.

## Arquivos canônicos

- `.hermes.md` — bootstrap Hermes específico do repositório.
- `ENTRYPOINT.md` — primeiro documento Project Atlas.
- `AGENTS.md` — invariantes operacionais compartilhados.
- `.ai/orchestration/hermes-policy.json` — bridge e enforcement Hermes.
- `.ai/orchestration/model-policy.json` — tiers, effort e sensibilidade.
- `.ai/orchestration/model-catalog.json` — allowlist fechada provider/model.
- `.ai/orchestration/model-routing.json` — rotas por Atlas Agent.
- `.ai/orchestration/fallbacks.json` — fallback baseado em evidência.

## Recursos nativos do Hermes relevantes

Hermes mantém sua configuração em `~/.hermes/config.yaml` (ou no diretório do profile ativo), suporta seleção explícita de provider/model, reasoning effort, auxiliary models e delegation.

Para uma execução pontual, o CLI suporta selecionar modelo e provider explicitamente. O projeto não deve depender de `provider: auto`.

Profiles Hermes também podem possuir `config.yaml`, `.env` e identidade próprios. Recomenda-se um profile dedicado ao CineTube para evitar defaults globais contaminarem a execução do projeto.

## Não confundir provider Atlas com provider ID Hermes

Os providers da política CineTube são lógicos e vinculados ao serviço contratado:

### Google

- Gemini 3.7 Flash
- Gemini 3.1 Pro

### OpenCode

- DeepSeek V4 Flash Free
- MiMo V2.5 Free
- Big Pickle

### Command Code

- Muse Spark 1.2 Contributor
- DeepSeek V4 Flash
- MiniMax M3
- GPT-5.6 Luna
- GLM 5.2

Hermes possui seu próprio registry de providers. Um nome parecido **não prova equivalência de serviço**.

Regras:

1. `google` Atlas pode ser ligado ao provider Gemini nativo do Hermes somente após confirmar conta, endpoint e modelo corretos.
2. `opencode` Atlas só pode ser ligado a um provider Hermes/OpenCode após confirmar que ele representa exatamente a oferta OpenCode definida no catálogo CineTube.
3. `command-code` não deve ser transformado em OpenRouter, Nous Portal ou outro agregador apenas porque hospeda o mesmo modelo.
4. Se Command Code exigir custom provider/base URL/plugin, configure esse mapping explicitamente fora do repositório e documente a identidade resultante.
5. Se a instalação Hermes não conseguir executar exatamente o provider/model aprovado, **não execute aquela rota no Hermes**.

Falha:

`HERMES_NO_APPROVED_MODEL_PROVIDER_AVAILABLE`

## Reasoning effort

Atlas define:

- Command Code / GPT-5.6 Luna -> `extra-high` sempre.
- demais pagos -> `high` sempre.
- modelos gratuitos -> máximo suportado pela rota real.

Hermes usa sua própria nomenclatura de reasoning effort. Para esta integração:

```text
Atlas high        -> Hermes high
Atlas extra-high  -> Hermes xhigh
Atlas max         -> maior nível realmente suportado pelo modelo/provider
```

Não reduzir effort para economizar. A economia vem de T0, tier mais barato, menor contexto, cache, reuso de evidência e stop-on-pass.

## Profile recomendado

Crie um profile Hermes dedicado, por exemplo `cinetube`, e configure seu `terminal.cwd` para a raiz local do repositório.

O profile deve:

- ter apenas providers realmente necessários/configurados;
- nunca depender de `provider: auto` para uma execução aceita como evidência Atlas;
- não conter um fallback global que ultrapasse `model-catalog.json`;
- manter segredos em `.env`/mecanismo de credenciais do Hermes, nunca no repositório;
- permitir que a sessão seja iniciada já com o provider/model resolvido pelo Atlas.

Não versionar API keys ou credenciais no CineTube.

## Preflight obrigatório

Antes de iniciar uma sessão model-backed ou delegation, determinar:

```text
GOAL_OR_TASK_ID
ATLAS_AGENT
ATLAS_TIER
LOGICAL_PROVIDER
HERMES_RUNTIME_PROVIDER
MODEL
ATLAS_EFFORT
HERMES_REASONING_EFFORT
SENSITIVITY
ACCEPTANCE_EVIDENCE
```

Fluxo:

```text
Goal/Task
  -> classificar risco e sensibilidade
  -> selecionar Atlas Agent
  -> tentar T0
  -> resolver Tier
  -> model-routing.json
  -> model-catalog.json
  -> mapear provider lógico para provider Hermes verificado
  -> aplicar effort
  -> iniciar sessão/delegation explicitamente
  -> executar
  -> validar evidência
  -> parar ou aplicar fallback Atlas
```

Campo não resolvido:

`HERMES_PREFLIGHT_FAILED`

## Execução principal

O main model do Hermes processa o loop principal de conversa/ferramentas. Por isso, a sessão deve começar com o provider/model já resolvido.

Não iniciar com um modelo genérico para depois pedir no prompt que ele "finja" usar outro modelo.

O prompt não altera a identidade real do runtime.

Se o modelo precisar mudar durante a tarefa, reexecute o preflight e valide a nova combinação antes da troca.

## Delegation

Hermes pode configurar provider/model/reasoning do subagente separadamente. Essa capacidade é útil para o Atlas, mas não autoriza seleção autônoma.

Antes de delegar:

1. resolver o Atlas Agent da subtarefa;
2. resolver Tier e rota;
3. configurar provider/model/effort da delegation explicitamente;
4. validar sensibilidade;
5. impedir fallback implícito para o main model caso essa identidade não seja uma rota Atlas válida;
6. registrar a identidade real na evidência.

Se o subagente herdar ou trocar para uma identidade não aprovada:

`HERMES_DELEGATION_POLICY_VIOLATION`

A saída não pode fechar acceptance criteria do Goal.

## Auxiliary models

Hermes pode usar modelos auxiliares para tarefas como compression, vision, web extraction, approval, MCP routing, session title e skill search.

Eles também consomem tokens/custo e também estão sujeitos à allowlist.

Princípio:

```text
auxiliary task
  -> T0 se possível
  -> cheapest approved safe route
  -> never unlisted provider/model
```

Orientação inicial:

- compression: modelo aprovado barato e seguro para o contexto;
- vision: rota multimodal aprovada, normalmente Google/Gemini quando a task exigir imagem;
- web extraction: rota aprovada barata;
- approval/review: modelo compatível com o risco da decisão;
- MCP routing, session title, skill search: preferir T0 ou rota aprovada mínima.

Não deixar auxiliary fallback escapar silenciosamente para um modelo principal não permitido pela rota específica.

## Fallback

Fallback pertence ao Project Atlas, não ao provider `auto` do Hermes.

Quando uma tentativa falhar:

1. obter evidência objetiva da falha;
2. consultar `fallbacks.json`;
3. resolver o próximo provider/model permitido;
4. validar mapping Hermes;
5. expandir somente contexto faltante;
6. executar uma tentativa séria;
7. parar quando a evidência passar.

Não escalar porque um modelo declarou insegurança.

## Contexto sensível

Antes de selecionar qualquer rota, aplicar `model-policy.json`.

As rotas marcadas como restricted-data não podem receber conteúdo sensível. Se houver dúvida razoável sobre a classificação, tratar como sensível.

A configuração Hermes, delegation ou auxiliary model não pode contornar essa regra.

## T5

T5 continua exigindo independência real entre providers conforme a política Atlas.

Dois modelos diferentes acessados pelo mesmo provider não substituem a exigência cross-provider quando o Goal a requer.

Se não for possível obter a independência exigida:

`CROSS_PROVIDER_VERIFICATION_UNAVAILABLE`

## Evidência obrigatória

Registrar por chamada aceita:

- Goal/Task ID;
- Atlas Agent;
- Tier;
- provider lógico Atlas;
- provider runtime Hermes real;
- model real;
- effort real;
- sensibilidade;
- tokens/custo quando disponíveis;
- fallback reason;
- evidência antes/depois;
- resultado.

Se a identidade real não puder ser comprovada:

`HERMES_MODEL_PROVIDER_POLICY_VIOLATION`

## Critério para usar Hermes no CineTube

Hermes pode executar uma tarefa somente se:

- `.hermes.md` foi carregado;
- `ENTRYPOINT.md` foi seguido;
- Goal ativo foi resolvido;
- provider/model Atlas foi escolhido antes da execução;
- existe mapping Hermes explícito e verificado;
- effort está correto;
- sensibilidade foi aplicada;
- automação/delegation não introduziu fallback não autorizado;
- evidência final registra a identidade real.

Caso contrário, abortar em vez de improvisar.

## Fontes oficiais Hermes a rever quando atualizar a ferramenta

A integração depende de comportamentos do Hermes que podem evoluir. Ao atualizar Hermes, revisar na documentação oficial:

- Context Files (`.hermes.md`, `AGENTS.md`);
- Configuration (`config.yaml`, providers, reasoning effort);
- Configuring Models (main vs auxiliary models);
- CLI (`--model`, `--provider`, `/model`);
- Profiles;
- Delegation e fallback behavior.

Mudanças nesses contratos exigem Documentation Delta nesta integração.

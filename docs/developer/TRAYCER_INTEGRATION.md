# Integração Traycer — Enforcement de Modelos no CineTube

## Objetivo

Impedir que o Traycer ou um coding agent acionado por ele troque silenciosamente o modelo, o provider ou o reasoning effort definidos pelo Project Atlas.

Para o CineTube, um modelo não é identificado apenas pelo nome. A identidade operacional é:

`provider + model + effort`

Exemplo válido:

`command-code / GPT-5.6 Luna / extra-high`

`GPT-5.6 Luna` por outro provider não é equivalente para fins desta política.

## Por que `AGENTS.md` sozinho não basta

O Traycer suporta `AGENTS.md` e o detecta automaticamente ao gerar/executar artefatos. Isso torna `AGENTS.md` a ponte correta entre Traycer e Project Atlas, mas ele é uma fonte de instruções, não uma garantia de que a plataforma realmente iniciou o modelo/provider solicitado.

O Traycer também possui seleção de modelos por **Model Profiles** (incluindo perfis customizados) e possui **Custom Hand-off Templates**. Portanto, a política do CineTube precisa ser aplicada em três camadas:

1. `AGENTS.md` — regra persistente e automaticamente descoberta pelo Traycer;
2. Custom Model Profile — controla modelos usados internamente pelo próprio Traycer quando a opção exata estiver disponível;
3. Custom Hand-off Template + configuração do coding agent — controla e valida o modelo/provider da execução entregue ao agente externo.

Fontes oficiais consultadas:

- Traycer changelog v2.11.6: suporte e detecção automática de `AGENTS.md`.
- Traycer changelog v2.10.10: Custom Hand-off Templates.
- Traycer changelog v2.15.13: Model Selection e Custom Model Profiles.
- Traycer changelog v2.15.0: Planner/Reviewer modes com modelo, reasoning setup e system prompt próprios.
- Traycer changelog v2.15.6: mecanismo de instruções extras para Custom CLI Agents.

A documentação pública atual não documenta um arquivo versionável do tipo `.traycer.json`/`.traycerrc` capaz de impor no repositório toda a seleção de Model Profile. **Não inventar um formato de configuração Traycer.** Os manifests `.ai/orchestration/*` são política Project Atlas; Traycer só os respeitará porque `AGENTS.md` e o handoff exigem sua leitura.

## Fontes canônicas

A integração deve obedecer, nesta ordem:

1. `AGENTS.md`
2. `.ai/orchestration/traycer-policy.json`
3. `.ai/orchestration/model-policy.json`
4. `.ai/orchestration/model-catalog.json`
5. `.ai/orchestration/model-routing.json`
6. `.ai/orchestration/fallbacks.json`
7. `.ai/orchestration/traycer-handoff-template.md`
8. Goal/Task ativo e contexto mínimo requerido

A política versionada no repositório vence qualquer preferência automática do Traycer, template antigo ou instrução casual de uma Task.

## Allowlist fechada

Somente estes pares estão autorizados:

### Google

- `google / Gemini 3.7 Flash`
- `google / Gemini 3.1 Pro`

### OpenCode

- `opencode / DeepSeek V4 Flash Free`
- `opencode / MiMo V2.5 Free`
- `opencode / Big Pickle`

### Command Code

- `command-code / Muse Spark 1.2 Contributor`
- `command-code / DeepSeek V4 Flash`
- `command-code / MiniMax M3`
- `command-code / GPT-5.6 Luna`
- `command-code / GLM 5.2`

A lista acima é informativa; a fonte machine-readable continua sendo `model-catalog.json`.

## Configuração obrigatória no Traycer

### 1. Confirmar leitura do `AGENTS.md`

O repositório deve estar aberto pela raiz do CineTube, com `AGENTS.md` visível no workspace. Não mover as regras Traycer para um prompt isolado e assumir que isso substitui o arquivo.

### 2. Criar um Custom Model Profile para o CineTube

Crie um perfil específico, por exemplo `CineTube Strict`.

Para cada etapa interna do Traycer que permita selecionar modelo/reasoning (Planner, Reviewer ou equivalente):

- escolha somente um modelo presente na allowlist;
- respeite o effort fixado pelo Atlas;
- não habilite substituição automática por outro modelo;
- se a UI do Traycer não disponibilizar exatamente um modelo aprovado para aquela etapa, **não use aquela etapa do Traycer neste projeto**.

Importante: um Custom Model Profile do Traycer controla modelos **internos do Traycer**. Ele não substitui a configuração do modelo do coding agent externo.

### 3. Configurar o coding agent/provider externamente

Antes do handoff, o agente de execução deve estar configurado no provider exato resolvido pelo Atlas:

- Google para os Gemini aprovados;
- OpenCode para Big Pickle, DeepSeek V4 Flash Free e MiMo V2.5 Free;
- Command Code para Muse Spark 1.2 Contributor, DeepSeek V4 Flash, MiniMax M3, GPT-5.6 Luna e GLM 5.2.

Um prompt não consegue transformar uma sessão já iniciada com o modelo errado no modelo correto. Se o host lançou o agente errado, a única ação válida é abortar e reiniciar/configurar o agente com o par correto.

### 4. Configurar o Custom Hand-off Template

Use `.ai/orchestration/traycer-handoff-template.md` como fonte canônica.

O Traycer permite templates de handoff personalizados; copie/adapte o conteúdo para o mecanismo de templates da instalação atual. Não altere semanticamente:

- allowlist fechada;
- provider binding;
- effort lock;
- sensitive-context gate;
- preflight;
- fail-closed;
- stop-on-pass;
- evidence-driven escalation.

A sintaxe exata de variáveis/template do Traycer pode mudar. Por isso o arquivo do repositório usa placeholders conceituais; eles precisam estar resolvidos antes da execução.

## Preflight obrigatório antes de qualquer handoff

Nenhum handoff é válido enquanto os oito campos abaixo não estiverem determinados:

```text
GOAL_OR_TASK_ID
ATLAS_AGENT
ATLAS_TIER
PROVIDER
MODEL
EFFORT
SENSITIVITY
ACCEPTANCE_EVIDENCE
```

Fluxo:

```text
Goal/Task
  -> classificar risco e sensibilidade
  -> escolher Agent
  -> T0 quando aplicável
  -> resolver Tier
  -> resolver provider/model no model-routing.json
  -> validar par no model-catalog.json
  -> aplicar effort do model-policy.json
  -> configurar/verificar coding agent no provider correto
  -> gerar handoff com todos os campos
  -> executar
  -> validar evidência
  -> parar ou usar fallback permitido
```

Se qualquer campo estiver ausente, retornar:

`TRAYCER_HANDOFF_PREFLIGHT_FAILED`

## Fail-closed

O Traycer não tem permissão para "salvar" uma execução escolhendo outro modelo.

São violações:

- Gemini solicitado no Google ser substituído pelo mesmo Gemini em agregador diferente;
- DeepSeek V4 Flash solicitado no Command Code ser aberto no OpenRouter ou outro provider;
- modelo indisponível ser substituído por uma versão anterior/nova não listada;
- `Auto`, `Smart`, `Best Model` ou roteamento automático escolher modelo fora da allowlist;
- Planner/Reviewer interno usar modelo não aprovado porque o Custom Profile não foi configurado;
- handoff omitir provider e informar somente o nome do modelo.

Quando não existir rota aprovada disponível:

`NO_APPROVED_MODEL_PROVIDER_AVAILABLE`

Não substituir silenciosamente.

## Reasoning effort

Regra fixa:

- Command Code / GPT-5.6 Luna -> `extra-high` sempre;
- demais modelos pagos -> `high` sempre;
- modelos gratuitos OpenCode -> máximo suportado sempre.

A economia deve vir de T0, escolha de tier/modelo, menor contexto, cache/reuso de evidência e `stop-on-pass` — nunca de reduzir effort após selecionar o modelo.

## Planner/Reviewer interno vs coding agent externo

Não confundir as camadas:

```text
Traycer
  |- Planner/Reviewer interno -> Custom Model Profile
  `- Handoff
       `- Coding Agent externo -> provider/model configurado no próprio agente/CLI
```

Uma rota como:

`implementer -> command-code / Muse Spark 1.2 Contributor`

não implica que o Planner interno do Traycer também seja Muse. Se o Planner fizer trabalho LLM para o CineTube, ele também precisa estar configurado com um modelo aprovado no Custom Profile.

## YOLO / automação contínua

Modos automáticos só podem ser usados quando preservarem o preflight em **cada** execução/handoff. Se o modo automático escolher modelos/providers sem expor ou respeitar o par resolvido pelo Atlas, ele deve ser desabilitado para o CineTube.

Nunca trocar previsibilidade por automação.

## Contexto sensível

Rotas restritas:

- OpenCode / DeepSeek V4 Flash Free
- OpenCode / MiMo V2.5 Free
- OpenCode / Big Pickle
- Command Code / Muse Spark 1.2 Contributor

Quando o contexto for classificado como sensível, o Traycer deve removê-las antes da seleção. A classificação e justificativas completas permanecem em `model-policy.json` e `model-catalog.json`.

## Fallback

Fallback é resolvido pelo Atlas, não pelo mecanismo automático do Traycer.

Antes de avançar para outro modelo:

1. confirmar falha objetiva ou risco que exija tier superior;
2. usar apenas o próximo par permitido em `fallbacks.json`/`model-routing.json`;
3. manter provider binding;
4. adicionar apenas o contexto/evidência faltante;
5. interromper se a evidência passar.

"O modelo parece inseguro" não é evidência.

## Diagnóstico quando Traycer usar o modelo errado

Verifique, nesta ordem:

1. o workspace foi aberto na raiz e o Traycer detectou `AGENTS.md`;
2. o Custom Model Profile correto está selecionado;
3. Planner/Reviewer não estão usando defaults Balanced/Frontier incompatíveis com a allowlist;
4. o Custom Hand-off Template ativo contém as regras do arquivo canônico;
5. o coding agent externo está realmente configurado para o provider/model do header;
6. algum modo automático está fazendo model/provider substitution;
7. o modelo estava indisponível e o Traycer aplicou fallback próprio em vez do `fallbacks.json`;
8. o log de execução registra o par real e não apenas o modelo solicitado.

Se o par real não puder ser comprovado, considere a execução inválida para fins de evidência Project Atlas.

## Critério de conformidade

Uma execução Traycer só pode ser aceita como evidência do Goal quando:

- preflight completo;
- provider/model real coincide com o resolvido;
- effort coincide com a política;
- sensibilidade foi respeitada;
- nenhum fallback não autorizado ocorreu;
- acceptance evidence foi registrada;
- resultado consta na telemetria.

Caso contrário, marque `MODEL_PROVIDER_POLICY_VIOLATION` e não use o output como evidência de conclusão.

## Manutenção

O Traycer evolui rapidamente. Ao atualizar a extensão, revisar especialmente:

- AGENTS.md support;
- Model Profiles;
- Planner/Reviewer model controls;
- Hand-off Templates;
- Custom CLI Agent instructions;
- comportamento de automação/YOLO;
- quaisquer novos mecanismos de configuração versionável.

Se o Traycer passar a oferecer um arquivo oficial de configuração versionado para Model Profiles/provider binding, ele pode complementar ou substituir parte da configuração manual somente após ADR e atualização desta política.

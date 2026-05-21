---
name: socrates
version: "2.1.0"
description: |
  Designa briefings estruturados e roteiros investigativos pra projetos da V4 Colli&Co. **4 modos canônicos** que cobrem cenários distintos de discovery na Fase 0 da Fundação Growth IA Ops v2.0.

  - **`roteiro-kickoff`** (default greenfield — Subfase 0.3): produz 4 outputs pré-kickoff: Briefing Inicial v0 + pasta de kickoff + `_evento.md` placeholder + Roteiro de Kickoff com 8 dimensões obrigatórias.
  - **`pos-kickoff`** (kickoff já aconteceu com transcrição em NotebookLM): cruza OSINT (F0-X do `public-context-curator`) × NotebookLM via MCP em matriz convergente/complementar/divergente/lacuna, produz `debriefing-pos-kickoff.md` (snapshot F0-Y com 15 seções) e opcionalmente promove pra Briefing v1. Captura sinais críticos que cliente ou OSINT isolados não produzem (validado no Onco Import 2026-05-13 — divergência stakeholders LinkedIn × reuniões, operações regulatórias não tratadas, claims problemáticos não auditados).
  - **`no-kickoff`** (cliente já roda há tempo, sem reunião de discovery formal — operador é informante primário): interroga operador em 10 blocos de perguntas + consome OSINT + consulta NotebookLM legado opcional, produz Briefing v1 direto sem snapshot intermediário.
  - **`briefing-cliente`** legado V4 Dante preservado em compatibilidade.

  Ative quando operador disser "preparar kickoff do projeto X" / "rodar socrates roteiro-kickoff" / "produzir briefing v0 e roteiro de kickoff" / "etapa 0.3 da Fase 0" (modo `roteiro-kickoff`), "consolidar debriefing pós-kickoff do projeto X" / "cruzar OSINT com NotebookLM" / "promover Briefing v0 para v1 usando NotebookLM" / "debriefing F0-Y do projeto X" (modo `pos-kickoff`), "cliente X não vai ter kickoff, gera briefing v1 direto" / "consolidar briefing inicial via interrogatório do operador" / "cliente já roda há tempo, monta briefing sem reunião" (modo `no-kickoff`).

  NÃO ative para: curadoria de UMA reunião isolada com transcrição (use `account-curator` — esta skill é síntese cross-reunião, account é curadoria por reunião); curadoria do pacote comercial pré-projeto Fase 0.1 (use `handoff-curator`); bootstrap de vault (use `vault-architect`); pesquisa de mercado profunda 4-dimensões (use `oraculo`); Briefing v0 standalone só com OSINT sem operador-input (use `public-context-curator` modo `no-kickoff` — entrega v0, esta skill entrega v1 com operador-input); diagnóstico de maturidade GTM (use `maturity-diagnoser`).
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, TodoWrite
mcp_requerido:
  - notebooklm   # cond. — obrigatório nos modos pos-kickoff e (opcional) no-kickoff
---

# socrates — Design investigativo de briefing e pergunta

Skill de **design investigativo**. Bounded Context: transforma contexto investigado em **documento estruturado de briefing** OU **roteiro de pergunta investigativa**. V1 V4 Dante reaproveitada e expandida em B2 do roadmap (Detalhamento da Fase 0) com **4 modos canônicos** cobrindo os cenários de discovery na Fase 0 (greenfield, kickoff-já-feito, cliente-em-curso).

**Princípio 1 (Especialização por Entrega):** critério de quebra **não** justifica skill nova — vocabulário e repertório dos outputs de todos os modos são homogêneos (todos consolidam contexto investigativo em briefing estruturado, variando apenas a fonte primária e o estado de entrada).

---

## Os 4 modos de operação

| Modo | Cenário de entrada | Fonte primária | O que produz | Linha de Visibilidade |
|---|---|---|---|---|
| **`roteiro-kickoff`** (default greenfield) | Vault inicializado pós-bootstrap + Handoff Operacional disponível + kickoff agendado | Handoff Operacional + claude.md + Tese GTM v2.2 | Briefing v0 + pasta de kickoff + `_evento.md` + Roteiro de Kickoff (4 outputs) | Roteiro: **Acima** (cliente vê) · demais: Abaixo |
| **`pos-kickoff`** (síntese cross-source) | Kickoff (e check-ins) já aconteceu com transcrição preservada em NotebookLM | OSINT (F0-X do `public-context-curator`) + NotebookLM via MCP | Debriefing F0-Y (`20-snapshots/YYYY-MM/debriefing-pos-kickoff.md`) + (cond.) Briefing v1 promovido | Abaixo |
| **`no-kickoff`** (sem reunião com cliente) | Cliente já roda há tempo, sem kickoff formal — operador é informante primário | Operador (interrogatório 10 blocos) + OSINT (cond.) + NotebookLM legado (cond.) | Briefing v1 direto (`10-fundacao/briefing-inicial.md` `version: pos-kickoff`, `via: no-kickoff`) | Abaixo |
| **`briefing-cliente`** (legado V4 Dante) | Insumos diversos do operador (notas, conversas, materiais soltos) | Insumos avulsos | Briefing estruturado em Markdown | Abaixo |

**Routing:** explícito pelo operador via prompt. Não há routing automático state-based — depende da intenção declarada. Default em projetos Growth IA Ops v2.0 greenfield: `roteiro-kickoff`. Default em retrofit de cliente legado: `no-kickoff` (se sem transcrição) ou `pos-kickoff` (se com transcrição em NotebookLM). Modo `briefing-cliente` disponível pra compatibilidade — ver [`references/modo-briefing-cliente-legado.md`](references/modo-briefing-cliente-legado.md).

### Fronteira com skills adjacentes (importante)

| Skill / modo | Output | Diferença |
|---|---|---|
| `public-context-curator` `no-kickoff` | Briefing **v0** (só OSINT) | Não consulta operador; entrega v0, não v1 |
| `socrates` `no-kickoff` (esta skill) | Briefing **v1** (OSINT + operador + NotebookLM cond.) | Operador é fonte primária; entrega v1 direto |
| `account-curator` `kickoff/recurring/qbr/...` | 3 arquivos em `70-memoria/YYYY-MM-DD-<evento>/` por reunião | Curadoria de UMA reunião — esta skill é síntese cross-reunião |
| `socrates` `pos-kickoff` (esta skill) | Debriefing F0-Y + Briefing v1 | Cross-source matrix OSINT × N reuniões via NotebookLM |

---

## Modo `roteiro-kickoff` — workflow operacional

### Pré-condições

- Vault inicializado (etapa 0.2 concluída — `vault-architect` modo `bootstrap` rodou e validação pós-bootstrap está ☑).
- `briefing-inicial.md` v0 ainda não existe (será produzido por esta skill).
- Pasta `40-comunicacoes/YYYY-MM-DD-kickoff/` ainda não existe (será criada por esta skill).
- Handoff Operacional disponível em `99-arquivo/handoff-comercial/YYYY-MM-DD-handoff-operacional.md` com `status: ready-to-bootstrap` (gate da `handoff-curator` fechado).
- `claude.md` da raiz do vault preenchido (lê pra calibrar tom e contexto).

### Inputs

| Input | Path | Obrigatório? |
|---|---|---|
| Handoff Operacional | `99-arquivo/handoff-comercial/YYYY-MM-DD-handoff-operacional.md` | Sim |
| `claude.md` raiz | `claude.md` | Sim |
| Tese de Maturidade GTM v2.2 | externa (referência) | Sim — define dimensões a investigar pra Pillars-Health |
| `data_kickoff_alvo` | string ISO date — informada pelo operador via prompt | Sim — nomeia a pasta `40-comunicacoes/YYYY-MM-DD-kickoff/` |

### Outputs (4)

| # | Output | Path canônico | Categoria | Linha de Visibilidade |
|---|--------|---------------|-----------|----------------------|
| 1 | **Briefing Inicial v0** | `10-fundacao/briefing-inicial.md` (frontmatter `version: pre-kickoff`) | 1 — Estado | Abaixo |
| 2 | **Pasta do evento de Kickoff** | `40-comunicacoes/YYYY-MM-DD-kickoff/` | 4 — Comunicação | Abaixo |
| 3 | **`_evento.md` placeholder** | `40-comunicacoes/YYYY-MM-DD-kickoff/_evento.md` | 4 — Comunicação | Abaixo |
| 4 | **Roteiro de Kickoff** | `40-comunicacoes/YYYY-MM-DD-kickoff/roteiro-kickoff.md` | 4 — Comunicação | **Acima** — enviado ao cliente |

Detalhamento de cada output: [`references/briefing-v0-template.md`](references/briefing-v0-template.md), [`references/evento-md-template.md`](references/evento-md-template.md), [`references/roteiro-kickoff-template.md`](references/roteiro-kickoff-template.md).

### Workflow operacional (7 passos)

#### Passo 1 — Validar pré-condições

1. Confirmar que vault Growth IA Ops v2.0 existe (presença de `claude.md` na raiz mencionando "Growth IA Ops v2.0").
2. Validar que `99-arquivo/handoff-comercial/YYYY-MM-DD-handoff-operacional.md` existe e tem `status: ready-to-bootstrap`. Se status for `pendente-acao-corretiva` ou `pendente-input-humano`, **recusar** e direcionar operador a fechar gate da `handoff-curator` antes.
3. Confirmar com operador a `data_kickoff_alvo` (data confirmada do encontro com cliente).

#### Passo 2 — Plano operacional (TodoWrite)

Use `TodoWrite` pra registrar os passos 3-7 + entrega final.

#### Passo 3 — Capture (extração investigativa)

Aplica o ciclo CODE (Capture/Organize/Distill/Express — Princípio 11). Para cada input lido com `Read`:
- **Handoff Operacional §§1-11:** entidades pra alimentar o Briefing v0 (10 seções).
- **Handoff Operacional §§12, 15, 16:** desvios cross-source, pendências abertas, inferências aplicadas — alimentam as 3 seções pós-roteiro do Roteiro de Kickoff.
- **`claude.md` raiz:** contexto do projeto pra calibrar tom + ponto focal pra calibrar formalidade da linguagem.
- **Tese GTM v2.2:** dimensões canônicas pra investigar Pillars-Health (4 pilares × 7 blocos × N1-N4).

#### Passo 4 — Compose Briefing Inicial v0

Escreva `10-fundacao/briefing-inicial.md` com frontmatter `version: pre-kickoff`, `status: draft`, `ticker`, tags canônicas. **10 seções** derivadas das §§1-11 do Handoff Operacional — schema completo em [`references/briefing-v0-template.md`](references/briefing-v0-template.md). Cada seção referencia explicitamente as §§ correspondentes do Handoff Operacional (rastreabilidade).

#### Passo 5 — Compose pasta de Kickoff + `_evento.md`

1. Criar pasta `40-comunicacoes/YYYY-MM-DD-kickoff/` (substituir `YYYY-MM-DD` pela `data_kickoff_alvo`).
2. Escrever `_evento.md` placeholder com metadados: data, hora, duração alvo (90 min), participantes cliente + operador, link do encontro, gravação acordada — schema em [`references/evento-md-template.md`](references/evento-md-template.md).

#### Passo 6 — Compose Roteiro de Kickoff

Escreva `40-comunicacoes/YYYY-MM-DD-kickoff/roteiro-kickoff.md` com frontmatter `output_type: roteiro-kickoff`, `status: rascunho`, e estrutura com **8 dimensões obrigatórias** (1 a 8) + abertura + encerramento + 3 seções pós-roteiro:

| Dim. | Duração | O que investiga | Apoia output downstream |
|---|---|---|---|
| 0. Abertura | 5 min | Apresentação + combinados de gravação/sigilo | — |
| 1. Negócio e contexto | 10 min | Receita, tamanho, concorrência, mudanças recentes | `cenario-baseline.md` (arquimedes) |
| 2. ICP / Cliente Ideal | 15 min | Melhor/pior cliente, comitê de decisão, dor primária | `icm.md` (michelangelo) |
| 3. Oferta e Posicionamento | 15 min | Pitch 30s, diferencial, oferta de ativação, pricing | `product-position.md` (da-vinci) |
| 4. Jornada e Funil | 10 min | Top fontes, conversões, durações, gargalo | `bpmn-bowtie.md` (revops-designer) |
| 5. Dados Disponíveis | 10 min | Mídia, CRM, analytics, BI | `measurement-plan.md` (measurement-architect) |
| 6. Restrições e Sensibilidades | 5 min | Marca, stakeholders, compliance | `brand-core.md` (campbell) |
| 7. Sucesso e Métricas | 10 min | 90d / 12m, North Star, owner | `gtm-plan.md` (cesar) |
| 8. Operação e Cadência | 10 min | Ponto focal, ritual, decisão, comunicação | `agents.md` do vault |
| 9. Encerramento | 5 min | Próximos passos | — |

**3 seções pós-roteiro (preenchidas pela skill):**
- **Validações pra fazer durante o Kickoff** — uma linha por inferência aplicada da §16 do Handoff Operacional.
- **Pendências do Handoff Operacional** — uma linha por item da §15 marcado "validar no Kickoff".
- **Validações de desvios cross-source** — uma linha por desvio Médio/Alto da §12 do Handoff Operacional.

Schema completo em [`references/roteiro-kickoff-template.md`](references/roteiro-kickoff-template.md). Detalhamento das 8 dimensões: [`references/dimensoes-kickoff.md`](references/dimensoes-kickoff.md).

#### Passo 7 — Verify (loop de feedback)

Antes de declarar conclusão, valide os 4 outputs contra critérios de qualidade abaixo. Se algum falhar, **NÃO** declare conclusão — itere com operador.

---

## Critérios de qualidade (gates)

### Briefing v0 passa quando

- ☐ Frontmatter completo (`version: pre-kickoff`, `ticker`, `status: draft`, tags canônicas).
- ☐ Todas as 10 seções preenchidas, com referências cruzadas explícitas pras §§ correspondentes do Handoff Operacional.
- ☐ Riscos e desvios Médio/Alto sumarizados em "Riscos identificados pré-kickoff".
- ☐ Inferências separadas de declarações (rastreabilidade).

### Roteiro passa quando

- ☐ Cobre **todas as 8 dimensões obrigatórias** (1 a 8).
- ☐ Cada inferência do Handoff Operacional (§16) tem pergunta correspondente em "Validações" (rastreabilidade).
- ☐ Cada pendência aberta (§15) tem entrada em "Pendências do Handoff Operacional".
- ☐ Cada desvio Médio/Alto da Análise de Desvio (§12) tem pergunta de validação no Kickoff (mesmo já tendo ação corretiva — Kickoff confirma alinhamento com cliente).
- ☐ Duração estimada está dentro do alvo (75-105 min).
- ☐ **Tom apropriado pra perfil do ponto focal — consultivo, não inquisitivo.**

### Pasta + `_evento.md` passa quando

- ☐ Data do Kickoff confirmada (substituiu `YYYY-MM-DD` placeholder no path).
- ☐ Participantes do lado cliente e do lado operador listados.
- ☐ Link do encontro (Meet/Zoom/presencial) registrado.
- ☐ Combinados de gravação acordados.

---

## Tom e linguagem do Roteiro

O Roteiro de Kickoff é o **único output da Fase 0 acima da Linha de Visibilidade** — chega ao cliente antes do encontro. Tom é crítico:

- **Consultivo, não inquisitivo.** Perguntas abrem espaço pra reflexão, não confrontam.
- **Calibrado pelo perfil do ponto focal.** Sócio fundador hands-on aceita perguntas mais diretas; CMO de empresa grande prefere abordagem mais formal. Skill lê `claude.md` pra calibrar.
- **Sem jargão interno V4.** Cliente não sabe o que é "Pillars-Health" ou "Bowtie BPMN" — perguntas traduzem o conceito pra linguagem do negócio.
- **Tempo estimado por dimensão** visível ao cliente — sinaliza profissionalismo e respeito pelo tempo dele.

---

## Handoff downstream

### Após etapa 0.3 (fim da Fase 0)

A Fase 0 fecha quando os 4 outputs do `socrates` são produzidos + Roteiro enviado ao cliente + data confirmada do Kickoff. Próxima etapa = **Subfase 1.1 (Kickoff)** da Fase 1 (Fundação).

### Status no `agents.md` do vault

- **`socrates`** muda de `not-configured` pra `running` (já produziu 4 outputs no modo `roteiro-kickoff`).
- **`handoff-curator`** muda de `not-configured` pra `archived` (skill é one-shot pré-projeto).

### Próxima skill ativa

Após o Kickoff (encontro real com cliente), `account-curator` é invocada com `evento_tipo: kickoff` em `70-memoria/YYYY-MM-DD-kickoff/transcricao.md` (transcrição da call) — produz curadoria + decisões detectadas + follow-up que alimentam o Briefing v1 (`version: pos-kickoff`).

### Versionamento do Briefing (pre-kickoff → pos-kickoff)

Mesmo arquivo (`10-fundacao/briefing-inicial.md`), mesmo path. Frontmatter evolui de `version: pre-kickoff` → `version: pos-kickoff` na Subfase 1.1 (operador atualiza com aprendizados do Kickoff + curadoria do `account-curator`). Histórico fica no Git. Princípio 6 (Versionamento em Três Camadas) + Princípio 8 (Vault como Camada de Estado — evergreen evolui, não é cópia paralela).

---

## Modo `pos-kickoff` — Síntese cross-source pós-discovery

> Detalhamento completo em [`references/modo-pos-kickoff.md`](references/modo-pos-kickoff.md).
> Schema F0-Y do output em [`references/debriefing-pos-kickoff-template.md`](references/debriefing-pos-kickoff-template.md).
> Bateria de queries em [`references/queries-notebooklm-bateria.md`](references/queries-notebooklm-bateria.md).

### Quando ativar

Cenário: Kickoff (e check-ins) **já aconteceram** com transcrição preservada em NotebookLM. Operador quer cruzar OSINT (F0-X) × transcrições em síntese consolidada que captura sinais críticos não-óbvios.

### Pré-condições

- Vault inicializado com `claude.md` raiz.
- `20-snapshots/YYYY-MM/contexto-inicial-cliente.md` (F0-X) existe.
- NotebookLM autenticado (`mcp__notebooklm__get_health` retorna `authenticated: true`).
- Notebook do cliente registrado e contém transcrições de Kickoff/cerimônias.

### Inputs

| Input | Path | Obrigatório? |
|---|---|---|
| OSINT F0-X | `20-snapshots/YYYY-MM/contexto-inicial-cliente.md` | Sim |
| NotebookLM ID | Biblioteca MCP | Sim |
| `claude.md` raiz | `claude.md` | Sim |
| Briefing v0 (se existir) | `10-fundacao/briefing-inicial.md` | Não — alimenta promoção pra v1 |
| Curadorias `account-curator` | `70-memoria/*/curadoria.md` | Não — bonus |

### Outputs

| # | Output | Path | Linha de Visibilidade |
|---|---|---|---|
| 1 | Debriefing F0-Y | `20-snapshots/YYYY-MM/debriefing-pos-kickoff.md` | Abaixo |
| 2 | (cond.) Briefing v1 | `10-fundacao/briefing-inicial.md` (`version: pos-kickoff`) | Abaixo |

### Workflow (7 passos)

1. **Validar pré-condições** — recusar se F0-X ausente ou NotebookLM não autenticado.
2. **Plano operacional** via `TodoWrite`.
3. **Capture OSINT** — ler F0-X integral, extrair §1, §2, §2.b, §3, §5, §6, §9, §10, §11, §12.
4. **Capture NotebookLM** — rodar bateria de 7 queries padronizadas + condicionais 8-10 conforme necessidade. Princípios: queries curtas, `source_format: footnotes` na 1ª e `none` nas seguintes, reutilizar `session_id`.
5. **(cond.) Capture material adicional** — curadorias do `account-curator` se existirem.
6. **Compose Debriefing** — escrever F0-Y com 15 seções. Aplicar matriz cross-source em §2, §7, §8, §12.
7. **Verify + (cond.) promoção pra v1** — gate V1-V10. Se prompt pediu promoção, atualizar Briefing v1.

### Gate de qualidade — Debriefing F0-Y passa quando

- ☐ Frontmatter completo (output_type, schema_id, ticker, fontes_primarias, linkbacks)
- ☐ Cruzamento OSINT × NotebookLM aplicado em ≥3 dimensões (§2 · §7 · §8 · §12)
- ☐ ≥5 sinalizações críticas em §1 ranqueadas por severidade — **nascidas de cruzamento**, não de fonte única
- ☐ Timeline em §6 com decisão + responsável + prazo (1 entrada por reunião disponível)
- ☐ §11 com ≥10 perguntas pra próxima call (filtra hipóteses do F0-X §11 que NotebookLM não cobriu)
- ☐ §12 com convergências + divergências + lacunas em colunas separadas
- ☐ §15 lista queries do NotebookLM + documentos-fonte + path do F0-X
- ☐ Sem afirmação factual sem fonte — [INFERÊNCIA] e [lacuna] explícitos
- ☐ Tom operador-only

---

## Modo `no-kickoff` — Briefing v1 direto sem reunião com cliente

> Detalhamento completo em [`references/modo-no-kickoff.md`](references/modo-no-kickoff.md).
> Bateria de perguntas em [`references/perguntas-operador-no-kickoff.md`](references/perguntas-operador-no-kickoff.md).

### Quando ativar

Cenário: cliente **já roda há tempo** na V4 (não é greenfield) e **NÃO vai haver kickoff formal de discovery**. Operador é o informante primário. Sócrates interroga o operador + consome OSINT (cond.) + consulta NotebookLM legado (cond.).

### Pré-condições

- Vault inicializado com `claude.md` raiz preenchido.
- Operador disponível pra interrogatório 45-90 min (confirmação explícita no prompt).
- (Recomendado) `20-snapshots/YYYY-MM/contexto-inicial-cliente.md` (F0-X) existe — se ausente, opera sem cruzamento OSINT (perde cobertura de §2, §7, §8, §12).
- (Opcional) NotebookLM com material legado registrado.

### Inputs

| Input | Path / origem | Obrigatório? |
|---|---|---|
| `claude.md` raiz | `claude.md` | Sim |
| Operador (informante) | Conversação interativa | **Sim — fonte primária** |
| OSINT F0-X | `20-snapshots/YYYY-MM/contexto-inicial-cliente.md` | Recomendado |
| NotebookLM ID | Biblioteca MCP | Opcional |

### Outputs

| # | Output | Path | Linha de Visibilidade |
|---|---|---|---|
| 1 | Briefing Inicial v1 | `10-fundacao/briefing-inicial.md` (`version: pos-kickoff`, `via: no-kickoff`) | Abaixo |

**Não produz snapshot intermediário** em `20-snapshots/` — vai direto pro evergreen.

### Workflow (7 passos)

1. **Validar pré-condições** — recusar se vault não inicializado ou operador indisponível.
2. **Plano operacional** via `TodoWrite`.
3. **(cond.) Capture OSINT** — se F0-X existe, ler e extrair itens que viram perguntas dirigidas ao operador.
4. **(cond.) Capture NotebookLM legado** — bateria reduzida (5 queries) focada em material acumulado.
5. **Interrogar operador (45-90 min)** — bateria de 10 blocos (A-J) com pré-pergunta de triagem. Pular perguntas já cobertas pelo OSINT/NotebookLM.
6. **Compose Briefing v1** — 10 seções espelhando template Briefing v0, com **marca de confiança por seção** (alta/média/baixa). Frontmatter `version: pos-kickoff`, `via: no-kickoff`, `fonte: socrates-no-kickoff`.
7. **Verify** — gate V1-V8.

### Gate de qualidade — Briefing v1 (`no-kickoff`) passa quando

- ☐ Frontmatter `version: pos-kickoff`, `via: no-kickoff`, `status: approved`, `fonte: socrates-no-kickoff`
- ☐ 10 seções preenchidas com marca de confiança (alta/média/baixa) por seção
- ☐ Todos os 10 blocos de perguntas operador rodaram (mesmo com "operador não sabe")
- ☐ §"Lacunas a validar com cliente" lista o que operador não respondeu + perguntas residuais do OSINT
- ☐ Stakeholders cross-source (LinkedIn do OSINT × operador) se OSINT disponível
- ☐ Claims problemáticos do OSINT — operador deu posição (ciente/não-ciente/disposição-pra-ajustar)
- ☐ Tom operador-only, sem perguntas dirigidas ao cliente

---

## Modo `briefing-cliente` (legado V4 Dante)

Modo legado preservado pra compatibilidade com workflows V4 Dante anteriores. Ativado por trigger explícito do operador (não default). Cobre o caso original da v1: organizar insumos diversos do operador (notas, conversas, materiais soltos) em briefing estruturado em Markdown.

**Status arquitetural:** preservado mas não documentado detalhadamente nesta skill. Refatoração formal reservada pra Bloco B7 (Catálogo canônico de outputs por skill). Detalhe operacional em [`references/modo-briefing-cliente-legado.md`](references/modo-briefing-cliente-legado.md).

---

## Anti-patterns

### Comuns aos 4 modos
- ❌ **Renomear paths canônicos.** `10-fundacao/briefing-inicial.md` e `40-comunicacoes/YYYY-MM-DD-kickoff/roteiro-kickoff.md` são fixos.
- ❌ **Atualizar `agents.md` ou `claude.md`.** Escopo da `vault-architect`. Skill só escreve nos paths do contrato.
- ❌ **Misturar Snapshot (Categoria 2) com Evergreen (Categoria 1).** F0-Y vive em `20-snapshots/` e envelhece; Briefing vive em `10-fundacao/` e evolui via Git no mesmo path.

### Específicos do `roteiro-kickoff`
- ❌ **Pular validação de pré-condição.** Se Handoff Operacional tem `status: pendente-acao-corretiva`, recuse — não produza Briefing v0 sobre base instável.
- ❌ **Inventar inferência.** Toda inferência vem do Handoff Operacional (§16). Skill não cria inferência nova — apenas propaga pra "Validações no Kickoff" do Roteiro.
- ❌ **Tom inquisitivo no Roteiro.** Cliente vê esse doc — perguntas são consultivas.
- ❌ **Jargão V4 no Roteiro.** "Pillars-Health", "Bowtie", "ICM", "PUV" são jargão interno. Traduza pra linguagem de negócio.
- ❌ **Misturar inferência com declaração no Briefing v0.** Cada seção referencia §§ do Handoff Operacional explicitamente.
- ❌ **Bypassar 8 dimensões obrigatórias.** Mesmo se algum dado já está no Handoff (ex: dimensão 5), pergunta de validação fica.

### Específicos do `pos-kickoff`
- ❌ **Pular OSINT.** Se F0-X não existir, recuse — oriente operador a rodar `public-context-curator` antes. Sem cruzamento, skill degrada pra simples extração de transcrição (use `account-curator`).
- ❌ **Inventar convergência ou divergência.** Cada linha de §2, §7, §8, §12 do F0-Y precisa de evidência rastreável nas duas fontes.
- ❌ **Tratar NotebookLM como única fonte de verdade.** Cliente pode errar fato sobre o próprio negócio — o OSINT confronta isso.
- ❌ **Cobrir reunião que não existe no notebook.** Listar fontes do notebook antes de assumir Kickoff/PE/Check-ins.
- ❌ **Mesclar Debriefing F0-Y com Briefing v1 num só arquivo.** São Categorias 2 vs 1 — papéis distintos.
- ❌ **Promover pra v1 sem aprovação do operador.** A promoção é decisão arquitetural — Briefing v1 vira fonte canônica das próximas skills.

### Específicos do `no-kickoff`
- ❌ **Pular interrogatório do operador.** Operador é fonte primária. Se for pra rodar só com OSINT, use `public-context-curator no-kickoff` (entrega v0). Se for pra rodar com transcrição, use `pos-kickoff`.
- ❌ **Inventar resposta quando operador disse "não sei".** Lacuna é dado — marca explícita.
- ❌ **Tom consultivo-cliente nas perguntas.** Operador é interno V4 — jargão liberado.
- ❌ **Produzir snapshot F0-Y aqui.** Esse modo vai direto pro evergreen.
- ❌ **Apagar Briefing v0 anterior antes de promover.** Preservar via Git (commit antes de sobrescrever).

---

## Linha de Visibilidade — resumo

```
═══════════════════════════════════════════════════
ACIMA DA LINHA (cliente vê)
═══════════════════════════════════════════════════
  • Roteiro de Kickoff (output 4)
  • Convite e agenda do Kickoff (operador envia separadamente)

───────────────────────────────────────────────────
ABAIXO DA LINHA (operador vê)
───────────────────────────────────────────────────
  • Briefing Inicial v0 (output 1)
  • Pasta de kickoff (output 2)
  • `_evento.md` placeholder (output 3)

───────────────────────────────────────────────────
SISTEMA (Claude Code + skills)
───────────────────────────────────────────────────
  • Skill `socrates` modo `roteiro-kickoff` rodando 1×
  • Inputs lidos: Handoff Operacional + claude.md + tese GTM v2.2
  • Git: commit "feat: briefing v0 + kickoff cerimonial preparados"
═══════════════════════════════════════════════════
```

---

## Referências

- Doc canônico de arquitetura: `c:\Users\mcafe\OneDrive\Documentos\Claude\Projects\06_teses\Tese Growth IA Ops v2.0\arquitetura\skills\setup\socrates.md`
- Tese Growth IA Ops v2.0 — Princípios 1, 4, 6, 8, 10, 11
- Tese de Maturidade GTM v2.2 — define dimensões a investigar pra Pillars-Health
- Fase 0 detalhada — etapa 0.3 invoca esta skill
- Skill V4 Dante original `/socrates` — base legada (modo `briefing-cliente`)
- [Eric Evans — DDD: Bounded Context](https://www.domainlanguage.com/ddd/) — fundamento do Princípio 4
- [Tiago Forte — CODE (Capture/Organize/Distill/Express)](https://www.buildingasecondbrain.com/) — ciclo cognitivo da skill (Princípio 11)

## References internas (carregadas sob demanda)

### Modo `roteiro-kickoff`
- [`references/dimensoes-kickoff.md`](references/dimensoes-kickoff.md) — detalhamento das 8 dimensões obrigatórias do Roteiro
- [`references/briefing-v0-template.md`](references/briefing-v0-template.md) — schema vazio do Briefing v0 (10 seções)
- [`references/roteiro-kickoff-template.md`](references/roteiro-kickoff-template.md) — schema vazio do Roteiro (8 dimensões + 3 pós-roteiro)
- [`references/evento-md-template.md`](references/evento-md-template.md) — schema vazio do `_evento.md` placeholder

### Modo `pos-kickoff` (v2.1)
- [`references/modo-pos-kickoff.md`](references/modo-pos-kickoff.md) — workflow detalhado + gates + anti-patterns
- [`references/debriefing-pos-kickoff-template.md`](references/debriefing-pos-kickoff-template.md) — schema F0-Y (15 seções + Verify gate V1-V10)
- [`references/queries-notebooklm-bateria.md`](references/queries-notebooklm-bateria.md) — bateria padronizada de 7 queries + condicionais

### Modo `no-kickoff` (v2.1)
- [`references/modo-no-kickoff.md`](references/modo-no-kickoff.md) — workflow detalhado + diferenças vs. `public-context-curator no-kickoff`
- [`references/perguntas-operador-no-kickoff.md`](references/perguntas-operador-no-kickoff.md) — bateria de 10 blocos (A-J) de interrogatório operador

### Modo `briefing-cliente`
- [`references/modo-briefing-cliente-legado.md`](references/modo-briefing-cliente-legado.md) — modo V4 Dante preservado em compatibilidade

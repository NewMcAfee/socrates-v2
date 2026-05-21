# Modo `pos-kickoff` — Síntese cross-source pós-discovery

> Reference da skill `socrates`. Detalhamento operacional do modo `pos-kickoff`.

---

## Quando ativar

Cenário canônico: **Kickoff (e check-ins subsequentes) já aconteceu(ram) com transcrição preservada em NotebookLM.** Operador quer consolidar tudo num documento único cruzando o OSINT pré-kickoff com o que o cliente efetivamente disse nas reuniões.

Ativadores explícitos do operador:
- "rodar socrates modo pos-kickoff pro cliente X"
- "consolidar debriefing pós-kickoff do projeto X"
- "cruzar OSINT com NotebookLM do cliente X"
- "promover Briefing v0 para v1 usando NotebookLM"
- "debriefing F0-Y do projeto X"

**Não ativar:**
- Cliente novo, kickoff vai acontecer no futuro → use `roteiro-kickoff`
- Não vai haver reunião com cliente → use `no-kickoff`
- Curadoria de uma reunião isolada → use `account-curator`
- Briefing v0 standalone só com OSINT → use `public-context-curator` modo `no-kickoff`

---

## Pré-condições

| # | Pré-condição | Como validar |
|---|---|---|
| 1 | Vault Growth IA Ops v2.0 inicializado | `claude.md` raiz menciona "Growth IA Ops v2.0" |
| 2 | `20-snapshots/YYYY-MM/contexto-inicial-cliente.md` (F0-X) existe | `Read` direto |
| 3 | NotebookLM do cliente registrado na biblioteca local | `mcp__notebooklm__list_notebooks` retorna o notebook |
| 4 | Notebook contém transcrições de Kickoff + check-ins disponíveis | `mcp__notebooklm__get_notebook` lista fontes |
| 5 | NotebookLM autenticado (`get_health` retorna `authenticated: true`) | `mcp__notebooklm__get_health` |
| 6 | (opcional) `10-fundacao/briefing-inicial.md` v0 existe — se sim, debriefing alimenta promoção pra v1 | `Read` direto |

Se **pré-condição 2 falhar** (sem F0-X), recusar: orientar operador a rodar `public-context-curator` antes.

Se **pré-condição 5 falhar**, rodar `mcp__notebooklm__setup_auth` (abre browser pra login Google).

---

## Inputs canônicos

| Input | Path / origem | Obrigatório? |
|---|---|---|
| OSINT pré-kickoff (F0-X) | `20-snapshots/YYYY-MM/contexto-inicial-cliente.md` | Sim |
| NotebookLM ID do cliente | Biblioteca local MCP (slug ou UUID) | Sim |
| Briefing v0 (se existir) | `10-fundacao/briefing-inicial.md` | Não — opcional |
| Curadorias do `account-curator` (se existirem) | `70-memoria/*/curadoria.md` | Não — bonus |
| `claude.md` raiz | `claude.md` | Sim — calibra tom |
| Janela temporal de interesse | string ISO (default = últimos 3 meses) | Sim |

---

## Outputs canônicos

| # | Output | Path | Categoria | Linha de Visibilidade |
|---|---|---|---|---|
| 1 | **Debriefing Pós-Kickoff** (F0-Y) | `20-snapshots/YYYY-MM/debriefing-pos-kickoff.md` | 2 — Snapshot | Abaixo |
| 2 | **Briefing v1 atualizado** (cond.) | `10-fundacao/briefing-inicial.md` (frontmatter `version: pos-kickoff`) | 1 — Estado | Abaixo |

**Output 1 sempre produzido.** Output 2 produzido se `prompt: promover-para-v1` declarado, OU se operador confirmar promoção no Passo 7.

Schema completo do Output 1: [`debriefing-pos-kickoff-template.md`](debriefing-pos-kickoff-template.md).

---

## Workflow operacional (7 passos)

### Passo 1 — Validar pré-condições

Aplicar checklist acima. Recusar com mensagem clara se 1, 2, 3, 4 ou 5 falhar.

### Passo 2 — Plano operacional (TodoWrite)

Registrar passos 3-7 + entrega final. Pelo menos: leitura OSINT · queries NotebookLM · cross-source matrix · escrita debriefing · gate Verify · (cond.) promoção pra v1.

### Passo 3 — Capture OSINT (extração investigativa)

Ler `20-snapshots/YYYY-MM/contexto-inicial-cliente.md` integral. Extrair em buffer interno:
- §1 Sumário Executivo + sinalizações críticas pré-kickoff
- §2 Identidade da empresa (campos + valores)
- §2.b Pessoas-chave (LinkedIn)
- §3 O que a empresa faz
- §4 Portfólio
- §5 Modelo de negócio
- §6 Claims auditados (🟢🟡🟠🔴)
- §9 Concorrência mapeada
- §10 Reputação online
- §11 Hipóteses pra discovery (vão alimentar §11 do debriefing)
- §12 Inconsistências (vão alimentar §12 do debriefing)

### Passo 4 — Capture NotebookLM (bateria de queries)

Selecionar notebook ativo via `mcp__notebooklm__select_notebook`.

Rodar bateria padronizada de 7 queries (detalhe em [`queries-notebooklm-bateria.md`](queries-notebooklm-bateria.md)):

1. Apresentação do cliente (modelo, mercado, ICP, PUV)
2. Canais de aquisição últimos N meses (gestão atual vs. anterior)
3. Decisões do Kick-off (data DD/MM/AA)
4. Decisões do Planejamento Estratégico (data DD/MM/AA)
5. Cada Check-in / cerimônia disponível (uma query por reunião)
6. Concorrentes citados pelo cliente
7. KPIs / OKRs / Budget

Queries condicionais (8-10) se necessário: stakeholders detalhados, stack técnica, pendências.

**Princípios operacionais:**
- Primeira query: `source_format: footnotes` (captura fontes/documentos do notebook)
- Demais: `source_format: none` (evita overlay travando o input)
- Reutilizar `session_id` quando possível; abandonar sessão se travar
- Tratamento de erros documentado em [`queries-notebooklm-bateria.md`](queries-notebooklm-bateria.md#tratamento-de-erros-do-mcp-notebooklm)

### Passo 5 — Capture material adicional (cond.)

Se existirem curadorias do `account-curator` em `70-memoria/*/curadoria.md`, ler as ≤6 mais recentes. Extrair: decisões fechadas, sinais, tom geral, riscos.

Se existir Briefing v0, ler estrutura — vai informar quais seções precisam evoluir pra v1.

### Passo 6 — Compose Debriefing (cross-source matrix)

Escrever `20-snapshots/YYYY-MM/debriefing-pos-kickoff.md` aplicando schema F0-Y ([`debriefing-pos-kickoff-template.md`](debriefing-pos-kickoff-template.md)).

**Matriz de cruzamento OSINT × NotebookLM** (aplicada em §2, §7, §8, §12 do output):

| Categoria | Sinal | Ação no output |
|---|---|---|
| **Convergente** | Mesma info nos dois lados | ✅ — confirma + marca confiança alta |
| **Complementar** | Um lado trouxe info que o outro não cobriu | 📎 — usa direto, marca origem |
| **Divergente** | Contradição factual entre as fontes | 🔴 — destaque em §12 + ação corretiva |
| **Lacuna** | Nenhum dos dois cobriu | 🟡 — registra em §14 (lacunas) |

Sinalizações críticas §1: **devem nascer de cruzamento** — não vale repetir o que está só num lado. Mínimo 5 itens com severidade 🔴/🟠/🟡.

§11 (pontos não tratados no kickoff): pegar §11 do F0-X (hipóteses pra discovery) e filtrar o que o NotebookLM **não cobriu** → vira pauta da próxima call.

§14 (lacunas): listar explicitamente itens que nem OSINT nem NotebookLM responderam.

### Passo 7 — Verify + (cond.) promoção pra v1

Aplicar gate V1-V10 do template. Se algum falhar, iterar — não declarar conclusão.

Atualizar `status` no frontmatter:
- `rascunho` (default) → operador revisa
- `aprovado` → após validação humana
- `promovido-evergreen` → após Briefing v1 atualizado

**Promoção pra v1** (cond. — só se operador confirmar OU prompt original pediu):
1. Ler Briefing v0 (se existir) ou criar do zero.
2. Atualizar frontmatter: `version: pos-kickoff`, `fonte: socrates-pos-kickoff`, `linkbacks.debriefing: 20-snapshots/YYYY-MM/debriefing-pos-kickoff.md`.
3. Reescrever seções do Briefing usando o Debriefing como insumo principal (10 seções espelhando §§ do F0-Y).
4. Atualizar `linkbacks.briefing_inicial` no Debriefing (rastreabilidade recíproca).
5. Mudar `status` do Debriefing pra `promovido-evergreen`.

---

## Gates de qualidade (Verify)

### Debriefing F0-Y passa quando

- ☐ Frontmatter completo (output_type, schema_id, ticker, fontes_primarias, linkbacks)
- ☐ Cruzamento OSINT × NotebookLM aplicado em ≥3 dimensões (§2 Identidade · §7 Concorrência · §8 Stakeholders · §12 Inconsistências)
- ☐ ≥5 sinalizações críticas em §1 ranqueadas por severidade
- ☐ Timeline das reuniões em §6 com decisão + responsável + prazo (1 entrada por reunião disponível no notebook)
- ☐ §11 (pontos não tratados) com ≥10 perguntas pra próxima call
- ☐ §12 (inconsistências) com convergências + divergências + lacuna em colunas separadas
- ☐ §15 (fontes) lista todas as queries do NotebookLM + documentos-fonte + path do F0-X
- ☐ Sem afirmação factual sem fonte — [INFERÊNCIA] e [lacuna] marcadas explicitamente
- ☐ Tom operador-only — sem perguntas dirigidas ao cliente, sem jargão de venda

### Briefing v1 (se promovido) passa quando

- ☐ Frontmatter atualizado: `version: pos-kickoff`, `status: approved`, `fonte: socrates-pos-kickoff`, `linkbacks.debriefing` preenchido
- ☐ 10 seções do template Briefing v0 ([`briefing-v0-template.md`](briefing-v0-template.md)) preenchidas com material atualizado pós-Kickoff
- ☐ §"Inferências a validar no Kickoff" do v0 reclassificada: cada inferência marcada como **confirmada** / **refutada** / **ainda em aberto** baseado no NotebookLM
- ☐ §"Riscos identificados" atualizada com riscos pós-discovery (§10 do Debriefing)
- ☐ §"Pendências abertas" atualizada com pendências ativas (§13 + §14 do Debriefing)

---

## Anti-patterns

Evitar:
- ❌ **Pular OSINT.** Se F0-X não existir, recusar — orientar a rodar `public-context-curator` antes. Sem OSINT, o cruzamento não existe e a skill degrada pra simples extração de transcrição (use `account-curator`).
- ❌ **Inventar convergência ou divergência.** Cada linha de §2, §7, §8, §12 precisa de evidência rastreável nas duas fontes.
- ❌ **Treat NotebookLM como única fonte de verdade.** Cliente erra fato sobre o próprio negócio — o OSINT confronta isso.
- ❌ **Cobrir reunião que não existe no notebook.** Listar fontes do notebook antes de assumir que tem Kickoff/PE/Check-ins.
- ❌ **Mesclar Debriefing com Briefing v1 num só arquivo.** São Categorias diferentes (2 Snapshot vs 1 Estado) com linhas de visibilidade idênticas mas papéis distintos. Snapshot envelhece — evergreen evolui no mesmo path.
- ❌ **Promover pra v1 sem aprovação do operador.** A promoção é decisão arquitetural — o Briefing v1 vira fonte canônica das próximas skills (`michelangelo`, `campbell`, `da-vinci`, `cesar`).
- ❌ **Sobrescrever curadorias de `account-curator`.** Esta skill **consome** as curadorias; nunca reescreve.

---

## Diferença vs. fluxo manual atual

Hoje (sem `pos-kickoff`):
1. `roteiro-kickoff` produz Briefing v0
2. Kickoff acontece
3. `account-curator` cura transcrição
4. **Operador manualmente** consome curadoria + Briefing v0 → atualiza Briefing v1
5. (Operador esquece de cruzar com OSINT, perde sinais críticos)

Com `pos-kickoff`:
1. `roteiro-kickoff` produz Briefing v0
2. Kickoff acontece (com transcrição em NotebookLM)
3. (opcional) `account-curator` cura transcrição
4. `socrates pos-kickoff` cruza OSINT × NotebookLM (+ opcional curadorias) → produz Debriefing F0-Y → promove Briefing v1 atualizado
5. Cruzamento sistemático garante captura de sinais críticos que humano esqueceria

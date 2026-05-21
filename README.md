# socrates v2 — Claude Code

Skill de **design investigativo** pra Claude Code. Transforma contexto investigado em **documento estruturado de briefing** OU **roteiro de pergunta investigativa**, cobrindo os 4 cenários canônicos de discovery na Fase 0 da Fundação Growth IA Ops v2.0 da V4 Colli&Co.

> **Versão:** v2.1.0 (estável, em produção).
> **Idioma:** pt-BR.
> **Stack:** Claude Code (CLI / desktop / extensão IDE).

---

## A skill

`socrates` é uma skill **única com 4 modos canônicos** — vocabulário e repertório dos outputs são homogêneos (todos consolidam contexto investigativo em briefing estruturado, variando apenas a fonte primária e o estado de entrada). Critério de quebra **não** justifica skill nova (Princípio 1 — Especialização por Entrega).

### Os 4 modos

| Modo | Cenário de entrada | Fonte primária | O que produz | Linha de Visibilidade |
|---|---|---|---|---|
| **`roteiro-kickoff`** (default greenfield) | Vault inicializado pós-bootstrap + Handoff Operacional disponível + kickoff agendado | Handoff Operacional + `claude.md` + Tese GTM v2.2 | Briefing v0 + pasta de Kickoff + `_evento.md` + Roteiro de Kickoff (4 outputs) | Roteiro: **Acima** (cliente vê) · demais: Abaixo |
| **`pos-kickoff`** (síntese cross-source) | Kickoff (e check-ins) já aconteceu com transcrição preservada em NotebookLM | OSINT (F0-X do `public-context-curator`) + NotebookLM via MCP | Debriefing F0-Y (`20-snapshots/YYYY-MM/debriefing-pos-kickoff.md`) + (cond.) Briefing v1 promovido | Abaixo |
| **`no-kickoff`** (sem reunião com cliente) | Cliente já roda há tempo, sem kickoff formal — operador é informante primário | Operador (interrogatório 10 blocos) + OSINT (cond.) + NotebookLM legado (cond.) | Briefing v1 direto (`10-fundacao/briefing-inicial.md` `version: pos-kickoff`, `via: no-kickoff`) | Abaixo |
| **`briefing-cliente`** (legado V4 Dante) | Insumos diversos do operador (notas, conversas, materiais soltos) | Insumos avulsos | Briefing estruturado em Markdown | Abaixo |

**Routing:** explícito pelo operador via prompt. Não há routing automático state-based — depende da intenção declarada. Default em projetos Growth IA Ops v2.0 greenfield: `roteiro-kickoff`. Default em retrofit de cliente legado: `no-kickoff` (se sem transcrição) ou `pos-kickoff` (se com transcrição em NotebookLM).

---

## Princípios canônicos

- **Diátaxis** — distingue Reference (template) de How-to (workflow operacional).
- **Linha de Visibilidade** — Roteiro é o único output da Fase 0 que chega ao cliente; tom é consultivo, não inquisitivo, e calibrado pelo perfil do ponto focal.
- **Rastreabilidade** — cada seção do Briefing v0 referencia §§ do Handoff Operacional; cada inferência aplicada (§16) vira pergunta de validação no Roteiro.
- **Versionamento em Três Camadas** — Briefing evolui no mesmo arquivo: `version: pre-kickoff` → `pos-kickoff` (histórico fica no Git, vault não duplica).
- **Cross-source matrix** (modo `pos-kickoff`) — cruza OSINT × NotebookLM em 4 categorias canônicas: convergente / complementar / divergente / lacuna. Captura sinais críticos que cliente ou OSINT isolados não produzem (validado no Onco Import 2026-05-13).
- **CODE cycle** (Capture / Organize / Distill / Express) aplicado em todos os modos pra extração investigativa.

---

## Modo `roteiro-kickoff` — 4 outputs canônicos

1. **Briefing Inicial v0** → `10-fundacao/briefing-inicial.md` (frontmatter `version: pre-kickoff`)
2. **Pasta do evento de Kickoff** → `40-comunicacoes/YYYY-MM-DD-kickoff/`
3. **`_evento.md` placeholder** → `40-comunicacoes/YYYY-MM-DD-kickoff/_evento.md`
4. **Roteiro de Kickoff** → `40-comunicacoes/YYYY-MM-DD-kickoff/roteiro-kickoff.md` (**Acima da Linha** — vai pro cliente)

### Roteiro de Kickoff — 8 dimensões obrigatórias

| Dim. | Duração | O que investiga | Apoia output downstream |
|---|---|---|---|
| 1. Negócio e contexto | 10 min | Receita, tamanho, concorrência, mudanças recentes | `cenario-baseline.md` (arquimedes) |
| 2. ICP / Cliente Ideal | 15 min | Melhor/pior cliente, comitê de decisão, dor primária | `icm.md` (michelangelo) |
| 3. Oferta e Posicionamento | 15 min | Pitch 30s, diferencial, oferta de ativação, pricing | `product-position.md` (da-vinci) |
| 4. Jornada e Funil | 10 min | Top fontes, conversões, durações, gargalo | `bpmn-bowtie.md` (revops-designer) |
| 5. Dados Disponíveis | 10 min | Mídia, CRM, analytics, BI | `measurement-plan.md` (measurement-architect) |
| 6. Restrições e Sensibilidades | 5 min | Marca, stakeholders, compliance | `brand-core.md` (campbell) |
| 7. Sucesso e Métricas | 10 min | 90d / 12m, North Star, owner | `gtm-plan.md` (cesar) |
| 8. Operação e Cadência | 10 min | Ponto focal, ritual, decisão, comunicação | `agents.md` do vault |

Duração total alvo: 75-105 min (incluindo abertura 5 min e encerramento 5 min).

---

## Modo `pos-kickoff` — Matriz cross-source

Depois que o Kickoff (e eventuais check-ins) já aconteceu e a transcrição está preservada em NotebookLM, `socrates pos-kickoff` cruza **OSINT × NotebookLM** em matriz canônica:

| Categoria | Significado | Ação |
|---|---|---|
| **Convergente** | OSINT e NotebookLM concordam | Promove pra Briefing v1 como fato consolidado |
| **Complementar** | OSINT cobre o que NotebookLM não cobre (ou vice-versa) | Promove ambos pra Briefing v1 |
| **Divergente** | OSINT e NotebookLM se contradizem | Sinaliza como **pendência crítica** pra validar com cliente (sinal mais valioso da matriz) |
| **Lacuna** | Nem OSINT nem NotebookLM cobrem | Sinaliza como gap pra discovery futuro |

Output: `20-snapshots/YYYY-MM/debriefing-pos-kickoff.md` (snapshot F0-Y com 15 seções) + opcionalmente promove Briefing v0 → v1.

---

## Modo `no-kickoff` — Retrofit sem reunião

Quando o cliente **já roda há tempo** e não vai ter kickoff formal — o operador é a fonte primária. Skill conduz **interrogatório em 10 blocos** estruturados, consome OSINT (cond.) e consulta NotebookLM legado (cond.), produzindo Briefing v1 direto (`version: pos-kickoff`, `via: no-kickoff`) sem passar por snapshot intermediário.

Bateria de perguntas em [`socrates/references/perguntas-operador-no-kickoff.md`](socrates/references/perguntas-operador-no-kickoff.md).

---

## Modo `briefing-cliente` (legado V4 Dante)

Compatibilidade com workflow v1 V4 Dante. Recebe insumos avulsos do operador (notas, conversas, materiais soltos) e produz briefing estruturado em Markdown. Detalhes em [`socrates/references/modo-briefing-cliente-legado.md`](socrates/references/modo-briefing-cliente-legado.md).

---

## Fronteira com skills adjacentes

| Skill / modo | Output | Diferença |
|---|---|---|
| `public-context-curator` `no-kickoff` | Briefing **v0** (só OSINT) | Não consulta operador; entrega v0, não v1 |
| `socrates` `no-kickoff` (esta skill) | Briefing **v1** (OSINT + operador + NotebookLM cond.) | Operador é fonte primária; entrega v1 direto |
| `account-curator` `kickoff/recurring/qbr/...` | 3 arquivos em `70-memoria/YYYY-MM-DD-<evento>/` por reunião | Curadoria de UMA reunião — `socrates` é síntese cross-reunião |
| `socrates` `pos-kickoff` (esta skill) | Debriefing F0-Y + Briefing v1 | Cross-source matrix OSINT × N reuniões via NotebookLM |
| `handoff-curator` | Handoff Operacional (pacote comercial pré-projeto Fase 0.1) | É **input** pro `socrates roteiro-kickoff` |
| `vault-architect` | Bootstrap do vault | Pré-condição pro `socrates roteiro-kickoff` |
| `oraculo` | Pesquisa de mercado profunda (4 dimensões) | Foco em mercado, não em cliente |
| `maturity-diagnoser` | Diagnóstico de maturidade GTM | Roda DEPOIS, em 1.1.5 — consome Briefing v1 |

---

## MCPs necessários

| Modo | MCP | Status |
|---|---|---|
| `roteiro-kickoff` | — | Nenhum requerido |
| `pos-kickoff` | **`notebooklm`** | Obrigatório (sem MCP, recusa rodar) |
| `no-kickoff` | `notebooklm` | Opcional (consome legado se disponível) |
| `briefing-cliente` | — | Nenhum requerido |

---

## Como invocar

No Claude Code, basta descrever a tarefa — a skill ativa sozinha pela `description` do frontmatter. Exemplos:

```
"preparar kickoff do projeto Loja X — rodar socrates roteiro-kickoff"
"produzir briefing v0 e roteiro de kickoff pro projeto Y, data alvo 2026-06-03"
"consolidar debriefing pós-kickoff do projeto X cruzando OSINT com NotebookLM"
"cliente Z não vai ter kickoff, gera briefing v1 direto via interrogatório"
"cliente W já roda há tempo, monta briefing sem reunião"
```

Você também pode invocar explicitamente:

```
"usa a skill socrates no modo pos-kickoff pra promover briefing v0 → v1 do projeto X"
```

---

## Validação empírica

- **Onco Import 2026-05-13** — modo `pos-kickoff` validado em projeto real. Matriz cross-source detectou divergências stakeholders LinkedIn × reuniões, operações regulatórias não tratadas no OSINT e claims problemáticos não auditados — sinais críticos que cliente OU OSINT isolados não produziriam.
- **Belô Café / Aquatro Suprimentos / Manchester** — modo `roteiro-kickoff` em produção em múltiplos clientes V4 Colli&Co.

---

## Estrutura do repo

```
socrates-v2/
├── README.md (este arquivo)
└── socrates/
    ├── SKILL.md
    └── references/
        ├── briefing-v0-template.md
        ├── debriefing-pos-kickoff-template.md
        ├── dimensoes-kickoff.md
        ├── evento-md-template.md
        ├── modo-briefing-cliente-legado.md
        ├── modo-no-kickoff.md
        ├── modo-pos-kickoff.md
        ├── perguntas-operador-no-kickoff.md
        ├── queries-notebooklm-bateria.md
        └── roteiro-kickoff-template.md
```

---

## Instalação local (Claude Code)

Clone (ou copie) `socrates/` pra dentro de `~/.claude/skills/`:

```bash
git clone https://github.com/NewMcAfee/socrates-v2.git
cp -r socrates-v2/socrates ~/.claude/skills/
```

A skill é ativada por `description` (sem comando explícito) — basta descrever a tarefa na sessão.

---

## Licença

Uso interno V4 Colli&Co. Para uso comercial externo ou redistribuição, consulte o autor.

# Template — Debriefing Pós-Kickoff (`20-snapshots/YYYY-MM/debriefing-pos-kickoff.md`)

> Reference da skill `socrates` modo `pos-kickoff`. Schema F0-Y v0.1.0 — snapshot Categoria 2 (datado, não evergreen).
>
> **Validado por execução real:** projeto Onco Import (2026-05-13) — cruzamento OSINT × NotebookLM gerou 3 sinalizações críticas que nenhuma fonte isolada produziria.

---

## Quando esse documento é gerado

- Cliente já realizou Kickoff (e opcionalmente check-ins/reuniões adicionais) com transcrição preservada em NotebookLM.
- `contexto-inicial-cliente.md` (F0-X) existe em `20-snapshots/YYYY-MM/` (produzido pelo `public-context-curator`).
- Briefing v0 pode ou não existir em `10-fundacao/briefing-inicial.md` — se existir, debriefing alimenta a promoção pra v1; se não existir, debriefing é o primeiro evergreen consolidado.

## Linha de Visibilidade

**Abaixo da linha** — operador-only. Não enviar ao cliente. É munição interna pra próximas skills (`michelangelo`, `campbell`, `da-vinci`, `cesar`).

---

```markdown
---
output_type: debriefing-pos-kickoff
schema_id: F0-Y
schema_version: "0.1.0"
ticker: {{ticker}}
cliente: {{cliente_nome_completo}}
modo: pos-kickoff
sub_modo: cross-source
janela_temporal: YYYY-MM-DD a YYYY-MM-DD
fontes_primarias:
  - notebooklm_id: {{uuid-do-notebook}}
  - notebooklm_local_id: {{slug-na-biblioteca-local}}
  - osint: 20-snapshots/YYYY-MM/contexto-inicial-cliente.md
notebooklm_consultas: {{n}}
data_consolidacao: YYYY-MM-DD
operador: {{email_operador}}
linkbacks:
  contexto_inicial_cliente: 20-snapshots/YYYY-MM/contexto-inicial-cliente.md
  briefing_inicial: null              # preenche quando promover pra v1
status: rascunho                       # rascunho | aprovado | promovido-evergreen
tags:
  - snapshot
  - fase-0
  - pos-kickoff
  - cross-source
  - {{TICKER}}
---

# Debriefing Pós-Kickoff — {{cliente_nome}}

> Cruzamento entre OSINT pré-kickoff (F0-X) × Transcrições reais via NotebookLM ({{lista-reunioes}}) · Data: YYYY-MM-DD · Operador-only · Não enviar ao cliente

---

## §1 Sumário Executivo

### O que aconteceu no projeto nos últimos {{N}} meses (em 1 parágrafo)

[Síntese cronológica e factual — quem cuidou antes, o que rodou, por que mudou, qual o estado atual. Cita números reais quando disponíveis.]

### Sinalizações críticas pós-kickoff (ranqueadas por severidade)

[Lista ≥5 itens com símbolo de severidade 🔴/🟠/🟡. **Cada item deve nascer do cruzamento OSINT × NotebookLM** — não vale repetir o que está só num lado. Formato:]

1. 🔴 **{{título-curto}}** — {{descrição com fontes cruzadas e impacto}}.
2. 🔴 **{{título}}** — {{descrição}}.
3. 🟠 **{{título}}** — {{descrição}}.
... (≥5 itens)

---

## §2 Identidade da Empresa — convergências e divergências cross-source

| Campo | OSINT (F0-X) | NotebookLM (cliente declarou) | Status |
|---|---|---|---|
| Setor | {{do osint}} | {{do nl}} | ✅ Convergente / ⚠️ Divergente / 🟡 Lacuna |
| Modelo (B2B/B2C/B2B2C) | | | |
| Tempo de mercado | | | |
| Tamanho equipe | | | |
| Ticket médio | | | |
| Faturamento | | | |
| Endereço | | | |
| Registros regulatórios | | | |
| ... | | | |

[Convergente = mesma info nos dois lados. Complementar = OSINT trouxe algo que cliente não declarou ou vice-versa. Divergente = contradição factual entre as fontes. Lacuna = nem OSINT nem cliente cobriram.]

---

## §3 ICP, Modelo de Negócio e Posicionamento (consolidado pós-kickoff)

### Modelo de receita declarado
[O que o cliente disse nas reuniões via NotebookLM.]

### ICP final aprovado
[Personas validadas + critérios MQL + perfil secundário + anti-ICP + descartes.]

### PUV / Posicionamento aprovado
[Versão final + decisões de edição registradas nas reuniões.]

### ⚠️ Pontos do OSINT NÃO discutidos nas reuniões
[Lista o que o OSINT viu (claims problemáticos, inconsistências factuais, tensões regulatórias) mas que o cliente não tocou.]

---

## §4 Estratégia atual acordada com a agência (quando aplicável)

### Budget
[Total mensal + distribuição por canal + autorização de teto.]

### Tese de funil
[Lógica de aquisição + qualificação + funil invertido/direto + negativações chave.]

### Metas declaradas
[OKRs trimestrais/anuais + KPIs alvo + performance atual se houver.]

---

## §5 Histórico das equipes anteriores

| Canal | Lógica | Resultado |
|---|---|---|
| Google Ads | | |
| Meta Ads | | |
| Orgânico | | |
| SEO | | |
| Outbound | | |
| ... | | |

### Motivo da troca
[Por que houve mudança de agência/gestor — declarado pelo cliente.]

### Cruzando com o OSINT
[O que o OSINT detectou como legado da equipe anterior (blog com personas IA, site desatualizado, claims órfãos).]

---

## §6 Decisões e pontos alinhados — timeline das reuniões

### 🗓️ {{Reunião 1}} — {{DD/MM/AA}}
- {{decisão}} — responsável: {{quem}} — prazo: {{quando}}

### 🗓️ {{Reunião 2}} — {{DD/MM/AA}}
...

---

## §7 Concorrência cross-source

### Concorrentes citados pelo CLIENTE (via NotebookLM)
[Lista direto do que o cliente disse.]

### Concorrentes mapeados pelo OSINT
[Lista do OSINT — pode ter overlap ou não com a do cliente.]

### ⚠️ Análise do não-overlap
[Quando cliente cita uns e OSINT outros: tem dois universos competitivos? Cliente não enxerga frente digital? Validar.]

---

## §8 Stakeholders mapeados — cross-source

### Lado Cliente — declarado nas reuniões (NotebookLM)
| Nome | Cargo declarado | Função na conta |
|---|---|---|
| | | |

### Lado Cliente — identificado pelo OSINT (LinkedIn)
| Nome | Cargo LinkedIn | Origem |
|---|---|---|
| | | |

### 🔴 Divergências
[Pessoas que estão num lado e não no outro — investigar.]

### Lado Operador / Agência
| Nome | Função na conta |
|---|---|
| | |

---

## §9 KPIs / OKRs / Budget — consolidado

[Detalhamento de metas trimestrais, CPL alvo, MQL/SQL, ciclo de venda, performance atual.]

### Marketing/Receita (sanity check)
[Razão entre budget de mídia e faturamento — sinal de subdimensionamento ou ajuste fino.]

---

## §10 Riscos e bloqueios ativos

| # | Risco / Bloqueio | Severidade | Origem | Mitigação em curso |
|---|---|---|---|---|
| 1 | | 🔴/🟠/🟡 | OSINT / NotebookLM / ambos | |

---

## §11 Pontos críticos pra próxima call (não tratados no kickoff)

[Pauta sugerida pra próxima reunião — derivada dos itens do OSINT que o cliente não cobriu nas reuniões. ≥10 perguntas organizadas por temas (Governança & Compliance · Marketing & Conteúdo · Comercial & Operação).]

---

## §12 Inconsistências cross-source

### Convergências (OSINT ↔ NotebookLM)
[Lista do que bate entre as duas fontes — calibra confiança.]

### Divergências / Tensões cross-source
[Lista do que contradiz — exige resolução.]

### Lacunas em ambas as fontes
[O que nem OSINT nem NotebookLM cobrem — vai pra discovery.]

---

## §13 Próximos passos imediatos

### Ações operacionais já priorizadas
[Itens do último check-in capturado via NotebookLM.]

### Ações adicionais sugeridas (pós-cruzamento)
[Itens novos que emergem do cross-source que não estavam nas reuniões.]

---

## §14 Lacunas

[Itens marcados explicitamente como lacuna — operador resolve em próxima rodada de discovery ou consulta lateral.]

---

## §15 Fontes consultadas

### NotebookLM ({{N}} consultas em {{data}} · sessão `{{slug}}`)
[Lista numerada das queries rodadas — vale como rastreabilidade.]

### Documentos-fonte do notebook
[Lista os arquivos-fonte que o notebook indexou — confere com `[1]`, `[2]`, etc. das citações.]

### Snapshot OSINT cruzado
- [`20-snapshots/YYYY-MM/contexto-inicial-cliente.md`](contexto-inicial-cliente.md) — schema F0-X v{{X.Y.Z}}

---

## Verify — gate de saída (V1-V10)

| # | Critério | Status |
|---|---|---|
| V1 | Frontmatter F0-Y + linkback para contexto-inicial-cliente.md | ☐ |
| V2 | Cruzamento OSINT × NotebookLM executado em ≥3 dimensões (identidade · ICP · concorrência · stakeholders) | ☐ |
| V3 | Sinalizações críticas pós-kickoff com severidade (≥5 ranqueadas) | ☐ |
| V4 | Timeline das reuniões com decisão · responsável · prazo | ☐ |
| V5 | Divergências cross-source explicitamente nomeadas em §12 | ☐ |
| V6 | Riscos/bloqueios em tabela com severidade e mitigação | ☐ |
| V7 | Pontos do OSINT não tratados no kickoff listados em §11 (≥10) | ☐ |
| V8 | Lacunas explícitas em §14 | ☐ |
| V9 | Próximos passos operacionais + adicionais sugeridos | ☐ |
| V10 | Sem afirmação factual sem fonte ([INFERÊNCIA] / [lacuna] marcadas) | ☐ |

**Gate fechado quando V1-V10 = ☑. Documento operador-only. Próximo passo: promover pra Briefing v1 (`10-fundacao/briefing-inicial.md` `version: pos-kickoff`) atualizando `linkbacks.briefing_inicial` neste snapshot.**
```

---

## Notas de uso

- **F0-Y é snapshot, não evergreen.** Vive em `20-snapshots/YYYY-MM/` — envelhece. A promoção pro evergreen acontece em `10-fundacao/briefing-inicial.md` com `version: pos-kickoff`.
- **Princípio 8 (canon):** o evergreen é o mesmo arquivo de v0 → v1 (versionamento via Git + frontmatter). Esse snapshot F0-Y é a **camada intermediária de auditoria** — não vira "v1.5".
- **Cross-source rigoroso:** §2, §7, §8 e §12 são as 4 seções onde o cruzamento gera mais valor. Validar que cada uma tem ≥1 linha de cada lado (OSINT + NotebookLM).
- **Severidade dos riscos:** 🔴 = blocker pra próxima fase / 🟠 = exige plano de mitigação / 🟡 = registrar e monitorar.
- **Tag canônica `cross-source`** facilita filtrar todos os debriefings produzidos por esse modo.
- **`status` evolui:** `rascunho` (recém-produzido) → `aprovado` (operador revisou) → `promovido-evergreen` (Briefing v1 atualizado).
- **Referência de execução validada:** `c:\Users\mcafe\OneDrive\Documentos\Claude\Projects\01_assessoria\Onco Import\20-snapshots\2026-05\debriefing-pos-kickoff.md` — usar como exemplo de boa execução.

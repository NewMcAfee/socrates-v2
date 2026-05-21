# Bateria padronizada de queries NotebookLM — modo `pos-kickoff`

> Reference da skill `socrates`. Carregada no Passo 4 do workflow do modo `pos-kickoff`.
>
> **Validado no projeto Onco Import (2026-05-13)** — 7 queries em sequência produziram material suficiente para o Debriefing F0-Y com 15 seções preenchidas.

---

## Princípios de query

1. **Curtas, focadas, uma dimensão por query** — queries longas e multi-dimensão fazem o NotebookLM retornar respostas truncadas ou genéricas.
2. **`source_format: footnotes` na primeira query da sessão** pra capturar fontes/documentos canônicos. Nas seguintes, `source_format: none` evita overlay de citação travando o input.
3. **Reutilizar `session_id`** quando possível — segunda pergunta consome contexto da primeira. Se a sessão travar (timeout, overlay), começar nova sessão e seguir.
4. **Mencionar a reunião pelo nome canônico** ("Kick-off de 05/03/26", "ROPRE Check-in de 22/04/26") em vez de termos vagos — o NotebookLM indexou por nome de arquivo.
5. **Sempre pedir resposta em pt-BR + marcar [lacuna] explicitamente** — evita alucinação preenchendo o vazio.

---

## Bateria de 7 queries

### Query 1 — Apresentação do cliente

```
Apresente o cliente {{nome_cliente}}: área de atuação, modelo de negócio (B2B/B2C/B2B2C), mercado/região, porte aproximado, produto/serviço principal, ICP e posicionamento declarado. Responda em pt-BR, citando fontes. Se algo não estiver no material, marque [lacuna].
```

Captura: §2 (Identidade) + §3 (ICP/Modelo/PUV).

---

### Query 2 — Canais e estratégia da gestão anterior

```
Quais canais de aquisição (Google Ads, Meta, SEO, orgânico, outbound) estavam rodando no projeto {{nome_cliente}} nos últimos {{N}} meses, e qual era a lógica/estratégia em cada um? Cite fontes em pt-BR.
```

Captura: §5 (Histórico das equipes anteriores).

---

### Query 3 — Kick-off

```
Quais foram as principais decisões e pontos alinhados no Kick-off de {{DD/MM/AA}} do projeto {{nome_cliente}}? Em pt-BR.
```

Captura: §6 (Timeline — primeira entrada).

---

### Query 4 — Planejamento Estratégico / reunião de aprovação

```
Quais foram as principais decisões aprovadas no Planejamento Estratégico de {{DD/MM/AA}} do projeto {{nome_cliente}}? Foque em: budget mensal e distribuição entre canais, campanhas aprovadas, ICP final, posicionamento (PUV), critério de qualificação de lead. Em pt-BR, resposta direta.
```

Captura: §4 (Estratégia atual acordada) + §6 (Timeline — segunda entrada) + §9 (KPIs/Budget).

---

### Query 5 — Check-ins / acompanhamento

```
Resuma o que foi tratado no {{nome_checkin}} de {{DD/MM/AA}} do projeto {{nome_cliente}} — status das campanhas, problemas observados e próximas ações. Em pt-BR.
```

Repetir para cada check-in disponível no notebook. Captura: §6 (Timeline) + §9 (Performance atual) + §10 (Riscos ativos) + §13 (Próximos passos).

---

### Query 6 — Concorrência

```
Quem são os principais CONCORRENTES do {{nome_cliente}} citados nas fontes, e qual a percepção do cliente sobre cada um (forças, fraquezas, diferenciação)? Em pt-BR.
```

Captura: §7 (Concorrência cross-source — lado cliente).

---

### Query 7 — KPIs e Metas

```
Quais são as METAS, KPIs e OKRs do projeto {{nome_cliente}} — incluindo metas trimestrais, CPL alvo, conversão de funil (MQL/SQL), faturamento atual e meta de crescimento, e budget mensal? Em pt-BR.
```

Captura: §9 (KPIs/OKRs/Budget consolidado).

---

## Queries condicionais (rodar se aplicável)

### Query 8 — Stakeholders detalhados

```
Liste todas as pessoas do lado cliente mencionadas nas reuniões do projeto {{nome_cliente}}, com cargo declarado e função na conta. Em pt-BR.
```

Roda se §8 (Stakeholders) tiver dúvida sobre quem é quem.

---

### Query 9 — Stack técnica e ferramentas

```
Quais ferramentas (CRM, BI, GTM, ads platforms, automation) o cliente {{nome_cliente}} usa, e qual o status de acesso/configuração de cada uma? Em pt-BR.
```

Roda se houver discussão sobre instrumentação ou tracking.

---

### Query 10 — Pendências e bloqueios atuais

```
Quais são os bloqueios, pendências e ações em aberto reportadas nas últimas reuniões do projeto {{nome_cliente}}? Quem é responsável e qual o status? Em pt-BR.
```

Captura: §10 + §13 + §14.

---

## Tratamento de erros do MCP NotebookLM

| Erro | O que fazer |
|---|---|
| `Timeout waiting for response from NotebookLM` | Retentar a mesma query depois de ~30s; se persistir, quebrar em duas queries menores. |
| `page.click: Timeout 30000ms` com `subtree intercepts pointer events` | Overlay de citação travou o input. Começar **nova sessão** (não passar `session_id`) e seguir. |
| `Failed to authenticate session` | Auth caiu. Rodar `mcp__notebooklm__setup_auth` e seguir. |
| `Browser page unresponsive: health check timed out` | Página presa. Começar nova sessão. |

---

## Como inferir os parâmetros do template

- `{{nome_cliente}}` — nome canônico do cliente como aparece nos arquivos do notebook
- `{{N}} meses` — janela de janela_temporal do frontmatter F0-Y (default: 3 meses)
- `{{DD/MM/AA}}` — datas das reuniões inferidas dos nomes dos arquivos no notebook (formato típico: `[Kick-off] Cliente - YYYY/MM/DD HH:MM`)
- `{{nome_checkin}}` — nome canônico da cerimônia (ROPRE Check-in Quinzenal, Check-in, Sprint Review)

Se o operador não souber as datas, listar os arquivos do notebook via `mcp__notebooklm__get_notebook` primeiro.

---

## Volume esperado

- **Free tier NotebookLM**: 50 queries/dia. Bateria padrão (7 queries + ~3 condicionais) = ~10 queries por projeto. Cabe folgado.
- **Tempo total**: ~5-10 min por execução completa (cada query roda em 30-90s).
- **Output bruto**: ~10-15 mil tokens de respostas — suficiente pra preencher as 15 seções do F0-Y sem citação trunca.

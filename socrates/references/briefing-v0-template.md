# Template — Briefing Inicial v0 (`10-fundacao/briefing-inicial.md`)

> Reference da skill `socrates` modo `roteiro-kickoff`. Schema vazio do Output 1 (Categoria 1 — Estado).

---

```markdown
---
output_type: briefing-inicial
version: pre-kickoff
status: draft
fonte: handoff-operacional@YYYY-MM-DD
created: YYYY-MM-DD
ticker: {{ticker}}
tags:
  - categoria/estado
  - fase/setup
  - status/draft
---

# Briefing Inicial — {{cliente_nome}}

> **Versão pre-kickoff (v0).** Derivado do Handoff Operacional auditado em {{data_handoff}}. Será enriquecido na Subfase 1.1 (Kickoff) e re-versionado pra `version: pos-kickoff` (mesmo arquivo, evolução via Git).
>
> Fonte canônica: [[99-arquivo/handoff-comercial/{{data}}-handoff-operacional]] (especialmente §§1-11).

## Identificação

[Espelha §1 do Handoff Operacional. Referência direta sem inferência.]

- **Cliente:** {{cliente_nome}}
- **Razão social:** {{razao_social}}
- **Ticker (Flow MCP):** {{ticker}}
- **Segmento:** {{segmento}}
- **Modelo de negócio:** {{modelo_negocio}}
- **Ticket médio:** {{ticket_medio}}
- **Ciclo de venda:** {{ciclo_venda}}
- **Ponto focal cliente:** {{ponto_focal_cliente}}
- **Ponto focal operador (Business Engineer):** {{ponto_focal_operador}}

## Contexto comercial

[Espelha §3 do Handoff Operacional — resumo executivo de 5-10 linhas. Como o cliente chegou, por que contratou agora, trigger event, caminho proposto.]

## Escopo contratado

[Espelha §4. Produtos/serviços contratados conforme contrato + cláusulas críticas relevantes pra Fundação.]

## Objetivos e expectativas declaradas

[Mescla §6 (promessas) e §7 (dores/objetivos priorizados). Inclui:]

- Como o cliente disse que vai medir sucesso (90d / 12m).
- Top 3 dores em ordem de severidade.
- Promessas críticas feitas durante a venda (com quem prometeu).

## Stakeholders

[Espelha §8. Comitê de decisão típico:]

| Nome | Cargo | Papel (decisor / influenciador / usuário / cético) | Sensibilidade |
|---|---|---|---|
| ... | ... | ... | ... |

## Stack técnico e dados disponíveis

[Mescla §9 (stack declarado) e §10 (dados disponíveis com status de acesso).]

## Restrições e sensibilidades

[Espelha §11. NDA, compliance, stakeholders sensíveis, temas a evitar.]

## Riscos identificados pré-kickoff

[Espelha §13 do Handoff Operacional + sumariza desvios Médio/Alto da §12.]

- **Risco 1:** {{descricao}} — fonte: {{fonte}} — severidade: {{alta/média/baixa}}.
- **Risco 2:** ...

> **Desvios cross-source ainda em monitoramento (§12 do Handoff Operacional):**
> - Eixo {{X}}: severidade {{medio/alto}} — ação corretiva {{status}} (ver §15 do Handoff).

## Inferências a validar no Kickoff

[Espelha §16 do Handoff Operacional. Cada inferência vira pergunta no Roteiro de Kickoff.]

| Campo | Valor inferido | Confiança | Fonte do palpite |
|---|---|---|---|
| ... | ... | alta/média/baixa | ... |

## Pendências abertas

[Espelha §15 do Handoff Operacional — itens marcados "validar no Kickoff" ou ainda não resolvidos.]

| # | Pendência | Owner | Como resolver |
|---|---|---|---|
| 1 | ... | ... | ... |

---

> **Próxima evolução do Briefing v0 → v1.** Três caminhos possíveis:
>
> 1. **Manual** (clássico): após o Kickoff (Subfase 1.1), `account-curator` cura a transcrição em `70-memoria/YYYY-MM-DD-kickoff/`. Operador atualiza este arquivo com aprendizados → `version: pos-kickoff` → `status: approved`.
> 2. **Automatizado via `socrates pos-kickoff`** (v2.1): cruza F0-X × NotebookLM com transcrição do Kickoff → produz Debriefing F0-Y → atualiza este arquivo com cruzamento sistemático cross-source.
> 3. **Via `socrates no-kickoff`** (v2.1, retrofit): cliente já roda há tempo, sem reunião de discovery — operador interroga e Sócrates consolida direto, sem passar pelo v0 manual.
```

---

## Notas de uso

- **Referência cruzada explícita:** cada seção espelha uma §§ do Handoff Operacional. Skill não inventa conteúdo.
- **Inferências separadas:** §16 inferências e §15 pendências ficam em seções dedicadas — nunca misturadas com declarações.
- **Frontmatter `status: draft`:** muda pra `approved` na Subfase 1.1 quando operador valida v1 (pós-kickoff).
- **Tags canônicas:** seguir convenção do `claude.md` do vault (`#categoria/estado`, `#fase/setup`, `#status/draft`).
- **Wikilink ao Handoff Operacional:** rastreabilidade obrigatória pra auditoria cruzada futura.

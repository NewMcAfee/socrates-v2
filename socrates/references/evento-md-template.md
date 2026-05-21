# Template — `_evento.md` placeholder

> Reference da skill `socrates` modo `roteiro-kickoff`. Schema vazio do Output 3 (Categoria 4 — Comunicação). Placeholder com metadados do evento — não enviado ao cliente (abaixo da linha).

Path canônico: `40-comunicacoes/YYYY-MM-DD-kickoff/_evento.md`.

---

```markdown
---
output_type: evento-cerimonial
evento_tipo: kickoff
status: agendado          # agendado | realizado | cancelado | reagendado
data: YYYY-MM-DD
hora: HH:MM
duracao_alvo_min: 90
participantes_cliente:
  - {{nome}} ({{cargo}}, {{email}})
participantes_operador:
  - {{nome}} (Business Engineer V4)
link_encontro: {{meet|zoom|presencial}}
gravacao_acordada: true   # true | false
tags:
  - categoria/comunicacao
  - fase/setup
  - status/agendado
---

# Evento — Kickoff — {{cliente_nome}}

## Pauta

Ver [[40-comunicacoes/{{YYYY-MM-DD}}-kickoff/roteiro-kickoff]] (Roteiro de Kickoff enviado ao cliente).

## Combinados pré-encontro

- Gravação: {{sim/não acordado}}
- Sigilo: stakeholders sensíveis identificados em §11 do Handoff Operacional ({{lista}})
- Tempo: encerrar {{horario_fim_strict}} {{ou flexivel}}
- Material complementar: {{deck/planilhas/outros}} a ser apresentado pelo cliente

## Transcrição

A ser produzida pós-evento e movida pra `70-memoria/YYYY-MM-DD-kickoff/transcricao.md`.

## Curadoria

A ser produzida pela skill `account-curator` em `70-memoria/YYYY-MM-DD-kickoff/curadoria.md` + `decisoes-detectadas.md` + `follow-up.md` (Subfase 1.1.2 do roadmap).

---

## Pós-evento (atualizar status do frontmatter)

Após realização do encontro:
- `status: realizado`.
- Data/hora reais (caso tenham mudado).
- Duração real do encontro.
- Participantes que efetivamente compareceram (vs. lista pré-evento).

Caso evento tenha sido cancelado ou reagendado:
- `status: cancelado` ou `status: reagendado`.
- Adicionar nota explicando motivo.
- Se reagendado, criar nova pasta `40-comunicacoes/YYYY-MM-DD-kickoff/` com nova data e marcar a antiga como `archived`.
```

---

## Notas de uso

- **Skill cria o `_evento.md` com `status: agendado`.** Status muda pra `realizado` pelo operador após o encontro.
- **Linha de Visibilidade:** abaixo (operador-only). Cliente recebe só o Roteiro de Kickoff.
- **Wikilink ao Roteiro:** rastreabilidade entre `_evento.md` e a pauta efetiva do encontro.
- **Transcrição vai pra `70-memoria/`, não fica em `40-comunicacoes/`.** Categoria 7 (Memória) é o destino canônico de transcrições brutas. `_evento.md` referencia o path destino.

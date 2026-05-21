# Modo `briefing-cliente` — Legado V4 Dante (compatibilidade)

> Reference da skill `socrates`. Documenta o modo legado preservado em compatibilidade.

## Status

**Modo preservado em compatibilidade**, não documentado em detalhes nesta versão. Refatoração formal reservada pra Bloco B7 (Catálogo canônico de outputs por skill).

Quando o catálogo formal de skills for definido em B7, decidir se:
- Mantém modo `briefing-cliente` ativo (com refatoração).
- Deprecia em favor de skill especializada nova.
- Reposiciona como sub-modo do `roteiro-kickoff` ou skill irmã.

Por enquanto: skill responde no modo legado quando operador invoca explicitamente.

---

## Quando ativar

Operador invoca explicitamente:
- "rodar socrates modo briefing-cliente"
- "produzir briefing genérico do projeto X" (sem referência a Handoff Operacional)
- "organizar insumos diversos em briefing estruturado"

NÃO ativar:
- Quando há Handoff Operacional disponível em `99-arquivo/handoff-comercial/` → usar modo `roteiro-kickoff` (default Growth IA Ops v2.0).
- Quando operador está em fluxo Fase 0 canônico — modo legado é fora de fluxo.

---

## Comportamento

Cobre o caso original da skill V1 V4 Dante:
- **Inputs:** insumos diversos do operador (notas, conversas, materiais soltos, transcrições parciais).
- **Output:** briefing estruturado em Markdown — sem path canônico fixo (operador define onde salvar).
- **Schema:** flexível, derivado do que os insumos cobrem. Skill organiza em seções razoáveis.

Comportamento canônico mantido conforme V1 — ver snapshot em `~/.claude/skills/_legado-dante-v1/socrates/SKILL.md`.

---

## Por que preservar?

1. **Compatibilidade** com workflows V4 Dante anteriores que ainda usam o modo.
2. **Transição** — durante migração de projetos legados (Manchester, Grupo Colina, etc.), pode haver chamadas residuais ao modo `briefing-cliente`.
3. **Uso ad-hoc** — operador pode precisar produzir briefing fora do fluxo canônico (ex: briefing pra reunião interna, briefing pra parceiro fora de projeto).

---

## Refatoração futura (B7)

Critérios pra avaliar destino do modo:
- **Manter** se houver demanda recorrente comprovada (≥3 projetos vivos usando).
- **Deprecar** se demanda for nula após 2-3 trimestres de Growth IA Ops v2.0 ativo.
- **Refatorar como skill irmã** (`briefing-builder` ou similar) se bounded context divergir significativamente do modo `roteiro-kickoff`.

Decisão registrada em [§5 do doc canônico de arquitetura](`c:\Users\mcafe\OneDrive\Documentos\Claude\Projects\06_teses\Tese Growth IA Ops v2.0\arquitetura\skills\setup\socrates.md`) (a tomar em B7).

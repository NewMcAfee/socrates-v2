# Template — Roteiro de Kickoff (`40-comunicacoes/YYYY-MM-DD-kickoff/roteiro-kickoff.md`)

> Reference da skill `socrates` modo `roteiro-kickoff`. Schema vazio do Output 4 (Categoria 4 — Comunicação). **Único output da Fase 0 acima da Linha de Visibilidade — chega ao cliente.**

---

```markdown
---
output_type: roteiro-kickoff
evento: kickoff
data_evento: YYYY-MM-DD
duracao_alvo: 90min
participantes_cliente:
  - {{nome}} ({{cargo}})
participantes_operador:
  - {{nome}} (Business Engineer V4)
versao: 1
status: rascunho       # ou: enviado-cliente
---

# Roteiro do Kickoff — {{cliente_nome}}

> **Objetivo do encontro:** alinhar contexto, validar inferências do handoff comercial, capturar dimensões críticas pra estruturação da Fundação do projeto.
>
> **Duração estimada:** 90 minutos.
> **Formato:** {{remoto via Meet/Zoom | presencial | híbrido}}.

---

## 0. Abertura (5 min)

- Apresentação do operador.
- Apresentação dos participantes do lado cliente.
- Combinados: gravação? sim/não. Sigilo de stakeholders sensíveis identificados.
- Validação de duração — encerrar 12h00 sharp ou tem flexibilidade?

## 1. Negócio e contexto (10 min)

- Como o negócio gera receita hoje? Quais são as principais linhas?
- Tamanho atual (ARR / GMV / faturamento aproximado)?
- Quantos clientes ativos? Crescimento últimos 12 meses?
- Quem são os 3 maiores concorrentes diretos? Em que vocês ganham, em que perdem?
- O que mudou nos últimos 6 meses que motivou procurar a V4 agora?

## 2. ICP / Cliente Ideal (15 min)

- Quem é o melhor cliente que vocês já tiveram? Por que ele é bom?
- Quem é o pior cliente que vocês já tiveram? Por que ele é ruim?
- Qual o comitê de decisão típico — quem influencia, quem assina, quem usa, quem reclama?
- Que dor específica faz ele procurar vocês?
- Por que clientes escolhem vocês vs. concorrência X?

## 3. Oferta e Posicionamento (15 min)

- Como vocês descrevem o produto/serviço em 30 segundos pra alguém que nunca ouviu falar?
- Que problema vocês resolvem que ninguém mais resolve da mesma forma?
- Por que clientes escolhem vocês?
- Existe uma oferta de ativação clara — bônus, stack de ofertas, descontos?
- Pricing é público ou sob consulta? Qual a estratégia?

## 4. Jornada e Funil (10 min)

- Como um lead chega hoje? Top 3 fontes ordenadas por volume.
- Vocês conseguem ver a taxa de conversão lead → MQL → SQL → cliente? Quanto é hoje?
- Quanto tempo dura cada etapa?
- Onde está o gargalo declarado pelo time?

## 5. Dados Disponíveis (10 min)

- Que ferramentas de mídia paga vocês têm acesso?
- CRM em uso? Há quanto tempo? Quantos contatos?
- Analytics configurado? Eventos custom? Server-side tagging?
- BI / dashboard centralizando dados? Onde fica?
- Vocês têm gravação de calls comerciais?

## 6. Restrições e Sensibilidades (5 min)

- Limites de exposição da marca — temas que não podem ser tocados?
- Stakeholders politicamente sensíveis internamente?
- Compliance, regulatório, jurídico (LGPD, CONAR, ANVISA)?

## 7. Sucesso e Métricas (10 min)

- Como vocês vão medir sucesso deste projeto em 90 dias? E em 12 meses?
- Existe métrica primária (North Star) já declarada? Ou precisamos construir?
- Quem é o responsável interno por bater essa métrica?

## 8. Operação e Cadência (10 min)

- Quem é o ponto focal cotidiano? Qual a disponibilidade?
- Cadência preferida de status report — semanal, quinzenal, mensal?
- Decisões grandes precisam passar por quem? Há comitê?
- Stack de comunicação preferido (Slack/WhatsApp/Email)?

## 9. Encerramento (5 min)

- Próximos passos: cronograma da Fundação, primeiro deliverable.
- Próxima conversa marcada.
- Combinados pós-Kickoff.

---

## Validações pra fazer durante o Kickoff

> Inferências aplicadas no Handoff Operacional (§16) que precisam ser confirmadas/refutadas durante o encontro:

| Campo inferido | Valor proposto | Como validar |
|---|---|---|
| {{exemplo: modelo_negocio}} | {{exemplo: b2b-saas}} | Pergunta direta na Dimensão 1 |
| ... | ... | ... |

## Pendências do Handoff Operacional (resolver no Kickoff)

> Itens marcados como "validar no Kickoff" na §15 do Handoff Operacional:

| Pendência | Owner | Como resolver |
|---|---|---|
| ... | ... | ... |

## Validações de desvios (cross-source)

> Cada desvio Médio/Alto da §12 do Handoff Operacional vira pergunta de confirmação com cliente:

| Desvio | Eixo | Severidade | Como validar |
|---|---|---|---|
| ... | Escopo / Resultado / Prazo / Resp / Próx passo | medio/alto | Pergunta na Dimensão correspondente + ação corretiva |

---

> **Como esse roteiro foi construído:** derivado do Handoff Operacional auditado em {{data_handoff}} ([[99-arquivo/handoff-comercial/{{data}}-handoff-operacional]]). Operador refina perguntas conforme perfil específico do ponto focal e contexto do encontro.
```

---

## Notas de uso

- **Tom consultivo, não inquisitivo.** Cliente vê esse documento — não pode parecer interrogatório.
- **Sem jargão V4.** Skill traduz "Pillars-Health" → "saúde dos pilares de crescimento" (ou termo equivalente em linguagem do negócio); "Bowtie BPMN" → "jornada do cliente"; "ICM" → "perfil do cliente ideal".
- **Tempo visível por dimensão.** Sinaliza profissionalismo + respeito ao tempo do cliente.
- **8 dimensões são obrigatórias.** Pular qualquer uma quebra outputs downstream da Fundação (ver matriz em [`dimensoes-kickoff.md`](dimensoes-kickoff.md)).
- **3 seções pós-roteiro** ficam visíveis no doc enviado ao cliente — sinalizam preparação rigorosa do operador.
- **Frontmatter `status: rascunho` → `enviado-cliente`** quando operador efetivamente envia (registra no email/WhatsApp).

## Personalização de tom

A skill lê `claude.md` do vault pra calibrar tom:
- Se ponto focal é sócio-fundador hands-on de empresa pequena → linguagem direta, exemplos numéricos, perguntas concretas.
- Se ponto focal é CMO de empresa grande → linguagem mais formal, contexto estratégico explícito, perguntas estruturadas.
- Se cliente migrou de outra agência → adicionar perguntas explícitas sobre o que herda/rejeita do operador anterior (ver `references/edge-cases.md` da `handoff-curator` §10.3).

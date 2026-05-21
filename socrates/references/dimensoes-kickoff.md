# 8 Dimensões Obrigatórias do Roteiro de Kickoff

> Reference da skill `socrates` modo `roteiro-kickoff`. Carregado no Passo 6 do workflow.

Cada dimensão tem: cobertura, perguntas-chave, output downstream da Fundação que ela apoia.

---

## Dimensão 1 — Negócio e contexto (10 min)

**Cobertura:** estrutura de receita, tamanho atual, concorrência, mudanças recentes que motivaram contratação.

**Perguntas-chave (consultivas):**
- Como o negócio gera receita hoje? Quais são as principais linhas?
- Tamanho atual (ARR/GMV/faturamento aproximado)?
- Quantos clientes ativos? Crescimento últimos 12 meses?
- Quem são os 3 maiores concorrentes diretos? Em que vocês ganham, em que vocês perdem?
- O que mudou nos últimos 6 meses que motivou procurar a V4 agora?

**Apoia downstream:**
- `arquimedes` → `cenario-baseline.md` (modelagem financeira + unit economics atual)
- `oraculo` → `relatorio-mercado.md` (concorrência + mudanças setoriais)

---

## Dimensão 2 — ICP / Cliente Ideal (15 min)

**Cobertura:** caracterização do melhor e do pior cliente, comitê de decisão, dor primária.

**Perguntas-chave:**
- Quem é o melhor cliente que vocês já tiveram? Por que ele é bom — fica mais tempo, gasta mais, indica mais?
- Quem é o pior cliente que vocês já tiveram? Por que ele é ruim — alto custo de servir, churn precoce, paga mal?
- Qual o comitê de decisão típico? Quem influencia, quem assina, quem usa, quem reclama?
- Que dor faz ele procurar vocês especificamente?
- O que faz ele escolher vocês em vez do competidor X?

**Apoia downstream:**
- `michelangelo` → `icm.md` (Ideal Customer Map com Anti-ICP + Champion identification + comitê de decisão)
- `da-vinci` → `icp-product-map.md`

---

## Dimensão 3 — Oferta e Posicionamento (15 min)

**Cobertura:** pitch, diferencial, oferta de ativação, pricing.

**Perguntas-chave:**
- Como vocês descrevem o produto/serviço em 30 segundos pra alguém que nunca ouviu falar?
- Que problema vocês resolvem que ninguém mais resolve da mesma forma?
- Por que clientes escolhem vocês vs. concorrência? Qual o motivo principal que ouvem do cliente?
- Existe uma oferta de ativação clara — algo que reduz fricção pra quem ainda não comprou? Bônus? Stack de ofertas?
- Pricing é público ou sob consulta? Qual a estratégia?

**Apoia downstream:**
- `da-vinci` → `product-position.md` (Proposta Única de Valor + Oferta de Ativação + Cross-Asset Map + Claim Audit)
- `escriba` → `copy-system.md` (fundação de voz e mensagem)

---

## Dimensão 4 — Jornada e Funil (10 min)

**Cobertura:** fontes de tráfego, conversões por etapa, gargalo declarado.

**Perguntas-chave:**
- Como um lead chega hoje? Quais são as top 3 fontes ordenadas por volume?
- Vocês conseguem ver a taxa de conversão entre lead → MQL → SQL → cliente? Quanto é hoje?
- Quanto tempo dura cada etapa (lead até primeira reunião, primeira reunião até proposta, proposta até assinatura)?
- Onde está o gargalo declarado pelo time hoje? O que machuca mais?

**Apoia downstream:**
- `revops-designer` → `bpmn-bowtie.md` (BPMN Bowtie top-of-funnel até pós-venda)
- `qualification-system-designer` → `sistema-qualificacao.md`
- `commercial-playbook-builder` → `playbook-comercial.md`

---

## Dimensão 5 — Dados Disponíveis (10 min)

**Cobertura:** ferramentas, histórico, instrumentação atual.

**Perguntas-chave:**
- Que ferramentas de mídia paga vocês têm acesso (contas Meta Ads, Google Ads, LinkedIn Ads)? Quem é o admin?
- CRM em uso? HubSpot? Salesforce? RD Station? Há quanto tempo? Quantos contatos?
- Analytics configurado? GA4? Eventos custom? Server-side tagging? CAPI?
- Existe BI / dashboard centralizando dados? Onde mora — Looker? Power BI? Planilha?
- Vocês têm gravação de calls comerciais? Onde? Em que formato?

**Apoia downstream:**
- `measurement-architect` → `measurement-plan.md` + `contrato-de-dados.md`
- `taxonomy-builder` → `taxonomia.yml`
- `newton` → `analise-dados-historicos.md` (se há dados)

---

## Dimensão 6 — Restrições e Sensibilidades (5 min)

**Cobertura:** marca, stakeholders sensíveis, compliance.

**Perguntas-chave:**
- Vocês têm limites de exposição da marca — temas que não podem ser tocados? Estilo que não combina?
- Existem stakeholders politicamente sensíveis internamente — pessoas que precisam ser consultadas antes de mudanças significativas?
- Compliance, regulatório, jurídico têm cláusulas que afetam o que pode ser feito (LGPD, CONAR, ANVISA)?

**Apoia downstream:**
- `campbell` → `brand-core.md` (Brand Strategy)
- `da-vinci` → `product-position.md` (Claim Audit)

---

## Dimensão 7 — Sucesso e Métricas (10 min)

**Cobertura:** definição de sucesso 90d / 12m, North Star, owner interno.

**Perguntas-chave:**
- Como vocês vão medir se este projeto deu certo em 90 dias? E em 12 meses?
- Existe uma métrica primária — North Star — que captura crescimento sustentável da operação? Já está declarada ou precisa construir?
- Quem é o responsável interno por bater essa métrica? Que recursos de pessoas/budget eles têm?

**Apoia downstream:**
- `cesar` → `gtm-plan.md` (4 Fits + North Star tree + hipóteses + sequência de loops)
- `arquimedes` → `cenario-baseline.md` (viabilidade financeira)

---

## Dimensão 8 — Operação e Cadência (10 min)

**Cobertura:** ponto focal, ritual, decisão, comunicação.

**Perguntas-chave:**
- Quem é o ponto focal cotidiano nosso lado de vocês? Qual a disponibilidade — quantas horas por semana?
- Cadência preferida de status report — semanal, quinzenal, mensal?
- Decisões grandes precisam passar por quem? Há comitê? Qual o tempo médio de aprovação?
- Stack de comunicação — Slack? WhatsApp? Email? Reunião gravada? Qual o formato preferido pra cada tipo de tema?

**Apoia downstream:**
- `agents.md` do vault — Status no projeto + ponto focal
- `claude.md` do vault — Contexto do projeto + cadência

---

## Cobertura matriz (8 dimensões × outputs Fundação)

| Dim. | oraculo | michelangelo | campbell | da-vinci | revops-designer | measurement-architect | newton | arquimedes | cesar | sobral |
|---|---|---|---|---|---|---|---|---|---|---|
| 1. Negócio | ●● | — | — | — | — | — | ● | ●●● | ● | ● |
| 2. ICP | ● | ●●● | — | ●● | ●● | ● | — | — | ● | ● |
| 3. Oferta | — | — | ● | ●●● | — | — | — | — | ●● | — |
| 4. Jornada | — | ● | — | — | ●●● | ●● | ● | — | — | ●● |
| 5. Dados | — | — | — | — | ● | ●●● | ●●● | — | — | ●● |
| 6. Restrições | ● | — | ●● | ●● | — | — | — | — | ● | — |
| 7. Sucesso | — | — | — | — | — | — | — | ●●● | ●●● | ● |
| 8. Operação | — | — | — | — | ●● | — | — | — | — | — |

(●●● = principal input · ●● = input significativo · ● = input contextual · — = não usa)

Nota: matriz mostra que **todas as 8 dimensões são necessárias** — pular uma quebra pelo menos 2 outputs downstream da Fundação. Por isso o Passo 6 do workflow exige cobertura completa.

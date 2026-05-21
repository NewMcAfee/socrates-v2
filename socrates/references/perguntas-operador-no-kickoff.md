# Bateria de perguntas ao OPERADOR — modo `no-kickoff`

> Reference da skill `socrates` modo `no-kickoff`. Carregada no Passo 4 do workflow.
>
> No modo `no-kickoff`, o cliente **não está disponível** para discovery formal. O operador (Business Engineer V4, account manager, gestor sênior) é o **informante principal** — quem detém o contexto vivo da conta via memória + material acumulado. Sócrates conduz interrogatório consultivo do operador.

---

## Princípios de interrogatório do operador

1. **Foco em lacunas, não em re-perguntar o que OSINT/NotebookLM já cobrem.** Antes de cada bloco de perguntas, identificar o que JÁ está coberto pelas fontes upstream e pular essas perguntas.
2. **Pergunta por bloco, não em ordem fixa.** Operador pode responder em ordem natural — Sócrates organiza depois.
3. **Tom direto e operacional, não consultivo-cliente.** Operador é interno V4 — não precisa traduzir jargão.
4. **Marcar "operador não sabe" como lacuna em vez de inventar.** Lacuna identificada vira pergunta pra próxima call cliente (ou pra arquivar como conhecida-desconhecida).
5. **Bloco "operador-only" é diferente de bloco "cliente-pra-confirmar"** — separar no output o que veio do operador (com confiança variável) vs. o que ainda precisa de validação do cliente.

---

## Pré-pergunta: triagem inicial

Antes de abrir os blocos, fazer 3 perguntas rápidas pro operador:

1. **Há quanto tempo a operação V4 está com esse cliente?** (calibra confiança do operador)
2. **Há quanto tempo o cliente existe?** (calibra escopo histórico das próximas perguntas)
3. **Qual a frequência atual de interação cliente-operador?** (semanal/quinzenal/mensal — afeta confiança em estado vigente)

Se operador entrou na conta há <30 dias, marcar `confianca_operador: baixa` no frontmatter e priorizar consulta ao NotebookLM/material legado em vez de operador.

---

## Bloco A — Negócio (espelha Dimensão 1 do `roteiro-kickoff`)

Pular perguntas já cobertas pelo OSINT (faturamento público / setor / produto).

- **A.1** Como o negócio gera receita HOJE (não no pitch comercial — na operação real)? Quais linhas dominam vs. residuais?
- **A.2** Tamanho real (faturamento mensal/anual) e crescimento últimos 12m — operador sabe ou é lacuna?
- **A.3** Quem são os 3 concorrentes que o cliente cita espontaneamente nas reuniões? (Lista pode divergir do OSINT — capturar ambas.)
- **A.4** O que mudou no negócio nos últimos 6m que está impactando o trabalho V4?

---

## Bloco B — ICP real vs. ICP declarado (espelha Dimensão 2)

- **B.1** Quem é o cliente que MAIS dá certo na operação atual — operador consegue caracterizar (porte/perfil/canal de origem/comportamento)?
- **B.2** Quem é o cliente que MAIS dá errado — churn precoce, alto custo de servir, paga mal?
- **B.3** Há divergência entre o ICP "oficial" (deck/site/handoff) e o ICP real (quem fecha)? Quanto?
- **B.4** Quem decide compra nesse mercado? Comitê típico? Operador conseguiu mapear na vivência?

---

## Bloco C — Oferta e Posicionamento (espelha Dimensão 3)

- **C.1** Qual o pitch que o time comercial do cliente USA hoje (não o que está no site)? Já mudou nos últimos 6m?
- **C.2** Existe diferenciação real vs. concorrência ou cliente vende preço? Operador testou hipóteses?
- **C.3** Há oferta de ativação clara? Bônus? Stack de ofertas? Funciona?
- **C.4** **Especial cross-OSINT:** o OSINT identificou claims problemáticos no site (claim "único" / "100%" / "+N anos" sem prova). Operador sabe se cliente está ciente? Há disposição pra ajustar?

---

## Bloco D — Funil e Histórico (espelha Dimensão 4)

- **D.1** Top 3 fontes de lead HOJE (volume real, não declarado)?
- **D.2** Taxa de conversão lead → MQL → SQL → cliente — operador tem número ou é lacuna?
- **D.3** Onde o time do cliente diz que dói mais? Operador concorda?
- **D.4** O que JÁ FOI TENTADO e fracassou na conta (canais, ofertas, abordagens)? Por quê?
- **D.5** Houve gestor anterior? Operador tem leitura do que ficou de legado bom vs. dívida técnica?

---

## Bloco E — Dados e Instrumentação (espelha Dimensão 5)

- **E.1** Stack atual: CRM em uso? Há quanto tempo? Acesso operador V4 tem ou não?
- **E.2** Analytics configurado (GA4 / events / CAPI / server-side)? Operador validou se funciona?
- **E.3** BI / dashboards centrais — onde mora? Operador acessa?
- **E.4** Mídia paga: contas + admin + budget mensal atual?
- **E.5** Há gravações de calls comerciais acumuladas? Onde? Quantas há disponíveis?
- **E.6** **Especial:** existe NotebookLM ou similar com transcrições? Quais cerimônias estão indexadas?

---

## Bloco F — Restrições e Sensibilidades (espelha Dimensão 6)

- **F.1** Marca: temas que não podem ser tocados? Estilo proibido?
- **F.2** Stakeholders sensíveis: quem precisa ser consultado antes de mudança grande?
- **F.3** Compliance/regulatório: LGPD/CONAR/ANVISA/CFM/CFC — operador identificou pontos de atenção?
- **F.4** **Cross-OSINT:** se OSINT detectou setor regulado + claims absolutos, operador trata como risco ativo?

---

## Bloco G — Sucesso e Métricas (espelha Dimensão 7)

- **G.1** Como cliente vai medir sucesso do projeto V4 em 90d? Em 12m?
- **G.2** North Star existe declarado, ou é lacuna a construir?
- **G.3** Quem é o responsável interno do cliente por bater essa métrica? Tem budget/poder?
- **G.4** Operador acha que a meta é realista? Otimista? Conservadora?

---

## Bloco H — Operação e Cadência (espelha Dimensão 8)

- **H.1** Ponto focal cotidiano do cliente: nome, cargo, disponibilidade real (horas/semana)?
- **H.2** Cadência de check-in atual: semanal / quinzenal / mensal?
- **H.3** Decisões grandes passam por quem? Comitê interno cliente? Tempo médio de aprovação?
- **H.4** Stack de comunicação: Slack / WhatsApp / Email? Operador tem grupo dedicado?
- **H.5** Próximas cerimônias agendadas: quais e quando?

---

## Bloco I — Riscos e Sinalizações (operador-only, sem espelho no roteiro-kickoff)

Esse bloco é **exclusivo do `no-kickoff`** — não tem equivalente nas 8 dimensões originais porque, em kickoff real, esses sinais emergem do cliente direto. Aqui o operador é a fonte.

- **I.1** Top 3 riscos da conta (financeiro / relacional / operacional) na opinião do operador?
- **I.2** Sinais de descontentamento cliente nos últimos 60 dias?
- **I.3** Há tensão com gestor anterior / dívidas operacionais herdadas que o cliente ainda referencia?
- **I.4** Probabilidade de renovação contrato em 12m, na percepção do operador?
- **I.5** **Cross-OSINT:** o OSINT detectou divergências/inconsistências que cliente ainda não tocou (ex: stakeholders LinkedIn ausentes das reuniões, operações regulatórias, claims problemáticos). Operador tem leitura?

---

## Bloco J — Lacunas e próxima call (fechamento)

- **J.1** Das perguntas A-I, quais o operador NÃO conseguiu responder? Lista pra próxima call cliente.
- **J.2** Tem alguma coisa importante sobre a conta que não foi tocada acima? Operador pode adicionar.
- **J.3** Cliente está disponível pra eventual call de validação posterior, ou modo `no-kickoff` é permanente?

---

## Como conduzir a bateria

1. **Apresentar ao operador a triagem inicial (3 perguntas)** — calibra o restante.
2. **Anunciar a estrutura:** "Vou rodar 10 blocos de perguntas (A-J). Cada bloco tem 3-6 perguntas. Posso ir um por vez ou prefere lista única pra responder corrido?"
3. **Default: um bloco por vez** — permite Sócrates aplicar follow-up adaptativo.
4. **Marcar lacuna explicitamente** quando operador disser "não sei" — não pressionar.
5. **Fechar com Bloco J** consolidando lacunas pra próxima ação.

---

## Volume esperado

- **Tempo total**: 45-90 min de conversação operador (depende da maturidade da conta + experiência do operador na conta).
- **Output bruto**: respostas estruturadas que alimentam direto as seções do Briefing v1.
- **Lacunas residuais**: típico 10-20% das perguntas viram lacuna explícita pra resolver depois.

---

## Diferença vs. Roteiro de Kickoff

| Aspecto | Roteiro de Kickoff (`roteiro-kickoff`) | Perguntas ao Operador (`no-kickoff`) |
|---|---|---|
| Destinatário | Cliente | Operador V4 (interno) |
| Tom | Consultivo, sem jargão V4 | Direto, operacional, jargão liberado |
| Linha de Visibilidade | Acima (cliente vê) | Abaixo (operador-only) |
| Tempo | 90 min | 45-90 min |
| Dimensões | 8 fixas | 10 blocos (8 espelhados + 1 risco + 1 fechamento) |
| Confiança nas respostas | Alta (cliente declara) | Variável — calibrada por triagem inicial |
| Lacunas | Validar na call | Marcar e arquivar pra próxima |

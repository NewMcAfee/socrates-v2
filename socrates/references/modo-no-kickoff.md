# Modo `no-kickoff` — Briefing v1 direto sem reunião com cliente

> Reference da skill `socrates`. Detalhamento operacional do modo `no-kickoff`.

---

## Quando ativar

Cenário canônico: **Cliente já está rodando há tempo na V4 (não é greenfield) e NÃO vai haver kickoff formal de discovery.** Operador é o informante principal — Sócrates interroga o operador, consome OSINT (se houver F0-X) e consulta NotebookLM (se houver material legado).

Ativadores explícitos do operador:
- "rodar socrates modo no-kickoff pro cliente X"
- "cliente X não vai ter kickoff, gera briefing v1 direto"
- "consolidar briefing inicial do cliente X via interrogatório"
- "cliente já roda há tempo, monta o briefing sem reunião"

**Não ativar:**
- Cliente novo, kickoff vai acontecer → use `roteiro-kickoff`
- Kickoff já aconteceu com transcrição → use `pos-kickoff`
- Operador quer só pesquisa pública sem operador-input → use `public-context-curator` modo `no-kickoff` (entrega v0, não v1)

---

## Fronteira crítica vs. `public-context-curator no-kickoff`

| Skill / modo | Inputs | Output | Consome operador? |
|---|---|---|---|
| `public-context-curator` `no-kickoff` | Só fontes públicas (web) | Briefing v0 (`version: pre-kickoff`) | Não |
| `socrates` `no-kickoff` (esta skill) | F0-X (do PCC) + NotebookLM (cond.) + **operador como informante** | Briefing v1 (`version: pos-kickoff`, `via: no-kickoff`) | **Sim — protagonista** |

Os dois coexistem. Em projeto com cliente legado, sequência canônica:
1. `public-context-curator no-kickoff` → produz F0-X + Briefing v0
2. `socrates no-kickoff` → consome F0-X + interroga operador → produz Briefing v1 (sobrescreve v0 no mesmo path)

Ou pular o passo 1 se operador já tem F0-X de outra origem.

---

## Pré-condições

| # | Pré-condição | Como validar | Recusa? |
|---|---|---|---|
| 1 | Vault Growth IA Ops v2.0 inicializado | `claude.md` raiz menciona "Growth IA Ops v2.0" | Sim |
| 2 | Operador disponível pra interrogatório 45-90 min | Confirmação explícita no prompt | Sim |
| 3 | `20-snapshots/YYYY-MM/contexto-inicial-cliente.md` (F0-X) existe | `Read` direto | Não — opera sem se aceitar perder cobertura |
| 4 | NotebookLM com material legado | `mcp__notebooklm__list_notebooks` | Não — opcional |
| 5 | `claude.md` raiz preenchido | `Read` direto | Sim |

**Pré-condição 3 não-fatal:** se OSINT não existir, skill avisa que vai operar sem cruzamento cross-source (perde §2, §7, §8, §12 de divergências) e oferece rodar `public-context-curator` antes.

**Pré-condição 4 não-fatal:** se não houver NotebookLM, skill opera 100% com operador-input + OSINT. Caracterização **`fonte: operador-only`** no frontmatter.

---

## Inputs canônicos

| Input | Path / origem | Obrigatório? |
|---|---|---|
| `claude.md` raiz | `claude.md` | Sim |
| OSINT (F0-X) | `20-snapshots/YYYY-MM/contexto-inicial-cliente.md` | Recomendado |
| NotebookLM ID do cliente | Biblioteca local MCP | Opcional |
| Operador (informante) | Conversação interativa | **Sim — fonte primária** |
| Briefing v0 (se existir) | `10-fundacao/briefing-inicial.md` | Não — operação substitui |

---

## Outputs canônicos

| # | Output | Path | Categoria | Linha de Visibilidade |
|---|---|---|---|---|
| 1 | **Briefing Inicial v1** | `10-fundacao/briefing-inicial.md` (frontmatter `version: pos-kickoff`, `via: no-kickoff`) | 1 — Estado | Abaixo |

**Diferença vs. modo `pos-kickoff`:** este modo **não produz snapshot intermediário** em `20-snapshots/`. Vai direto pro evergreen — porque o evergreen é a primeira materialização consolidada do contexto (não há transcrições para gerar um Debriefing intermediário rico).

---

## Workflow operacional (7 passos)

### Passo 1 — Validar pré-condições

Aplicar checklist acima. Recusar se 1, 2 ou 5 falhar. Pré-condições 3 e 4 são opcionais — avisar perda de cobertura se ausentes.

### Passo 2 — Plano operacional (TodoWrite)

Registrar passos 3-7 + entrega final. Pelo menos: leitura OSINT (cond.) · consulta NotebookLM (cond.) · triagem do operador · 10 blocos de perguntas · consolidação · Verify.

### Passo 3 — Capture OSINT (cond.)

Se F0-X existir, ler integralmente. Extrair em buffer: identidade, claims auditados, concorrência, inconsistências, hipóteses pra discovery.

Esses itens viram **lista de perguntas dirigidas** pra confirmar com o operador no Passo 5, em vez de virarem direto seções do Briefing — porque OSINT sozinho não cobre operação real, e operador pode contradizer ou completar.

### Passo 4 — Capture NotebookLM (cond.)

Se notebook do cliente registrado, rodar bateria reduzida de queries focadas em **material legado** (cliente já roda há tempo — transcrições antigas, decks históricos):

1. Resumo do cliente e operação atual conforme indexado no notebook
2. Histórico das equipes anteriores (se aplicável)
3. Decisões recentes registradas
4. Concorrentes citados pelo cliente em qualquer reunião
5. KPIs/metas declarados em qualquer doc do notebook

Não rodar a bateria completa do `pos-kickoff` — aqui o NotebookLM é input contextual, não fonte primária.

### Passo 5 — Interrogar operador (45-90 min)

Aplicar bateria de [`perguntas-operador-no-kickoff.md`](perguntas-operador-no-kickoff.md):

1. **Pré-pergunta — Triagem (3 perguntas)** — calibra confiança operador
2. **Bloco A — Negócio** (4 perguntas, espelha Dim. 1 do roteiro-kickoff)
3. **Bloco B — ICP real vs. declarado** (4 perguntas, espelha Dim. 2)
4. **Bloco C — Oferta e Posicionamento** (4 perguntas, espelha Dim. 3 + cross-OSINT claims)
5. **Bloco D — Funil e Histórico** (5 perguntas, espelha Dim. 4 + legado)
6. **Bloco E — Dados e Instrumentação** (6 perguntas, espelha Dim. 5)
7. **Bloco F — Restrições e Sensibilidades** (4 perguntas, espelha Dim. 6 + cross-OSINT)
8. **Bloco G — Sucesso e Métricas** (4 perguntas, espelha Dim. 7)
9. **Bloco H — Operação e Cadência** (5 perguntas, espelha Dim. 8)
10. **Bloco I — Riscos** (5 perguntas, exclusivo `no-kickoff`)
11. **Bloco J — Lacunas e fechamento** (3 perguntas)

**Princípios operacionais:**
- Default: um bloco por vez, com follow-up adaptativo
- Marcar lacuna explicitamente quando operador disser "não sei" — não pressionar
- Pular perguntas já cobertas integralmente pelo OSINT ou NotebookLM (com confirmação operador)
- Tom direto operador-V4 (sem traduzir jargão como faz no `roteiro-kickoff`)

### Passo 6 — Compose Briefing v1

Escrever `10-fundacao/briefing-inicial.md` aplicando schema do [`briefing-v0-template.md`](briefing-v0-template.md) com adaptações:

**Frontmatter atualizado:**
```yaml
output_type: briefing-inicial
version: pos-kickoff
status: approved
via: no-kickoff
fonte: socrates-no-kickoff
fontes_consultadas:
  - osint: 20-snapshots/YYYY-MM/contexto-inicial-cliente.md   # cond.
  - notebooklm: {{notebook_id}}                                # cond.
  - operador: {{email_operador}}
data_consolidacao: YYYY-MM-DD
ticker: {{ticker}}
linkbacks:
  contexto_inicial_cliente: 20-snapshots/YYYY-MM/contexto-inicial-cliente.md   # cond.
tags:
  - categoria/estado
  - fase/setup
  - status/approved
  - via/no-kickoff
```

**10 seções (espelham template Briefing v0)** preenchidas com cruzamento operador + OSINT + NotebookLM:

1. Identificação — predominantemente do OSINT (CNPJ, sede, AFE, etc.) + confirmação operador
2. Contexto comercial — predominantemente operador
3. Escopo contratado — predominantemente operador
4. Objetivos e expectativas declaradas — operador + cross-NotebookLM
5. Stakeholders — operador + cross-OSINT (LinkedIn)
6. Stack técnico e dados — operador (Bloco E)
7. Restrições e sensibilidades — operador + cross-OSINT (regulatório)
8. Riscos identificados — operador (Bloco I) + cross-OSINT (claims, inconsistências)
9. Inferências a validar — **renomear pra "Lacunas a validar com cliente"** — itens do Bloco J + perguntas pendentes
10. Pendências abertas — derivadas do Bloco D (o que ficou de gestão anterior) + Bloco J

**Diferenças do Briefing v0 produzido por `roteiro-kickoff`:**
- Seção "Inferências a validar no Kickoff" → renomeada pra "Lacunas a validar com cliente" (não há kickoff)
- Cada seção marca confiança: **alta** (operador disse + OSINT confirma), **média** (só operador OU só OSINT), **baixa** (operador disse "não sei" + OSINT lacuna)
- Frontmatter `version: pos-kickoff` direto (pula v0)

### Passo 7 — Verify

Aplicar gate V1-V8 abaixo. Se falhar, iterar com operador antes de declarar conclusão.

---

## Gates de qualidade (Verify)

### Briefing v1 produzido por `no-kickoff` passa quando

- ☐ Frontmatter `version: pos-kickoff`, `via: no-kickoff`, `status: approved`, `fonte: socrates-no-kickoff`
- ☐ 10 seções preenchidas, com **marca de confiança** por seção (alta/média/baixa)
- ☐ Todos os 10 blocos de perguntas operador rodaram (mesmo que algum tenha resposta "operador não sabe")
- ☐ §"Lacunas a validar com cliente" lista explicitamente o que operador não respondeu + perguntas residuais do OSINT
- ☐ Stakeholders cross-source — se OSINT tinha LinkedIn de pessoas que operador não conhece, marcado em §5
- ☐ Claims problemáticos do OSINT — operador deu posição (ciente/não-ciente/há disposição-pra-ajustar) em §7 ou §8
- ☐ Cross-NotebookLM (se rodou) — material legado citado em rodapé/inline onde relevante
- ☐ Tom operador-only, sem perguntas dirigidas ao cliente

---

## Anti-patterns

Evitar:
- ❌ **Pular interrogatório do operador.** Operador é fonte primária — se for pra rodar só com OSINT + NotebookLM, use `pos-kickoff` (cliente já se manifestou via transcrição) ou `public-context-curator no-kickoff` (OSINT puro).
- ❌ **Inventar resposta quando operador disse "não sei".** Marca lacuna explícita. Lacuna é dado.
- ❌ **Tom consultivo-cliente nas perguntas.** Operador é interno V4 — jargão liberado. Tom de roteiro-kickoff aqui é desperdício.
- ❌ **Tratar OSINT como secundário.** OSINT confronta operador — operador pode estar errado sobre algo factual do próprio cliente. Cross-source matrix gera valor.
- ❌ **Produzir snapshot F0-Y aqui.** Esse modo vai direto pro evergreen. F0-Y é exclusivo do `pos-kickoff`.
- ❌ **Apagar Briefing v0 anterior antes de promover.** Operador pode querer comparar — preservar via Git (commit antes de sobrescrever).

---

## Diferenças entre os 3 modos consultivos

| Aspecto | `roteiro-kickoff` | `pos-kickoff` | `no-kickoff` |
|---|---|---|---|
| Cliente disponível? | Vai estar (no kickoff futuro) | Já se manifestou (transcrições) | Não vai estar |
| Output principal | Briefing v0 + Roteiro | Debriefing F0-Y + Briefing v1 cond. | Briefing v1 direto |
| Fonte primária | Handoff Operacional | OSINT + NotebookLM | Operador |
| Tempo de execução | ~30-60 min | ~30-60 min | ~60-120 min (inclui interrogatório) |
| Linha de Visibilidade do main output | Acima (Roteiro vai ao cliente) | Abaixo | Abaixo |
| Versionamento Briefing | v0 → (depois) v1 | v0 → v1 (cruzado com NotebookLM) | direto v1 (sem v0) |

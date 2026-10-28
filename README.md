# FICHA DE DECISÕES · War Room FiapBank (CP6 · 3 aulas)

> Este arquivo é o **README.md do repositório do grupo** (`cp6-warroom-<nome-do-grupo>`).
> Vale **5,0 pontos** (rodadas 0,5 · relâmpagos 0,3), e a nota é pela
> **justificativa**, não pela letra. Preencham após cada aula e commitem até
> **23h59 do mesmo dia** (regras completas na seção 5 do enunciado).
>
> **Os incidentes da madrugada são revelados só em aula.** O título de cada registro
> será **ditado pelo professor na hora**; ninguém se antecipe.

**Grupo (nome da equipe plantonista):** BugKillers

**Turma:** 2CCPO **Repo:** `cp6-warroom-bugkillers`

**Integrantes (nome + RM):**

| Nome | RM |
|---|---|
| Gabrielly Lorentz | 565806 |
| Giovanna Praieiro | 565681 |
| Julia Aparicio | 563623 |
| Heitor Barbosa | 563078 |
| Maria Eduarda de Oliveira | 565386 |
| Nicolle Calasans | 564381 |

## 0. Setup do repositório (antes da 1ª aula; podem apagar esta seção depois)

1. Um integrante cria o repo **público** no GitHub: `cp6-warroom-<nome-do-grupo>`
   (ex.: `cp6-warroom-debugadores`), com um README qualquer
2. Substituam o conteúdo do `README.md` por este template (no navegador, pelo próprio
   GitHub, ou clonando):
   ```bash
   git clone https://github.com/<conta>/cp6-warroom-<nome-do-grupo>.git
   cd cp6-warroom-<nome-do-grupo>
   # substitua o conteúdo do README.md por este template e:
   git add .
   git commit -m "chore: ficha em branco do grupo"
   git push
   ```
3. Postem o **link no Teams** (o mesmo link para todo o grupo)

**Convenção de commits** (1 commit por rodada; relâmpagos podem ir junto com a
rodada seguinte):

```
decisao: R1 - opcao C (<resumo da justificativa em uma frase>)
decisao: R2 - opcao A (<resumo em uma frase>)
pos-mortem: relatorio de incidente da madrugada
```

---

## 📁 Dossiê técnico do FiapBank (MVP em produção)

**Stack:** Java 17 + Spring Boot + Spring Data JPA + Oracle. API com endpoints em
`/api/contas` e `/api/transferencias` (cenário visto desde a Aula 13).

**Contrato e regras de negócio que o banco prometeu aos clientes e aos reguladores:**

| Regra | Como deve ser |
|---|---|
| Transferência **PIX** | taxa **R$ 0,00** |
| Transferência **TED** | taxa fixa **R$ 5,00** |
| Saldo | **nunca fica negativo**: transferência/saque sem saldo é recusado com erro claro |
| Número de conta | **sequencial e único** (1001, 1002, 1003...), gerado pelo sistema |
| Extrato de transferência | grava **quem pagou** e **quem recebeu**, na ordem certa |
| CPF | **dado sensível**: nunca aparece nas respostas da API |
| Consultas ao banco | sempre parametrizadas, e cada operação **usa e libera** a conexão |
| Suíte de testes | roda antes de todo deploy; **verde** é pré-requisito pra subir |

**Como escrever a justificativa:** nomeie o **mecanismo técnico** em jogo (o
conceito das Aulas 11 a 15 que explica o incidente) e o **trade-off** (velocidade ×
segurança × faturamento × dívida). "Porque é mais seguro" não é justificativa.

---

# 📝 REGISTRO DE DECISÕES

> A cada incidente, o professor dita o título (ex.: "Rodada 1"). **Copiem o modelo
> abaixo, colem no fim desta seção** e preencham com o rascunho feito em aula, junto
> com o **placar do grupo** após a consequência. Commitem 1 commit por rodada até
> 23h59 do dia.
>
> **Eventos relâmpago:** registrem apenas se o grupo for **afetado** (o professor
> chama quem for; não ser chamado é bom sinal).

**Modelo (copiar para cada decisão):**

```
## <título ditado pelo professor>

**Tipo:** ( rodada / relâmpago ) · **Voto:** ( A / B / C / D )

**Justificativa:**

<2 a 3 linhas: mecanismo técnico + trade-off>

**Placar do grupo após esta decisão:** 🔥 __ · 💰 R$ __ mil · 🧹 __
```

*(as decisões entram aqui, na ordem em que a madrugada as trouxer; placar inicial:
🔥 7 · 💰 0 · 🧹 2)*

## RODADA 1: O saldo que virou negativo (19h12)

**Tipo:** rodada · **Voto:** C

**Justificativa:**

Corrigir a regra de negócio para impedir saldo negativo e adicionar um teste com `assertThrows`, garantindo que transferências sem saldo suficiente sejam recusadas. Embora exija mais trabalho imediato que um hotfix, a solução reduz o risco de recorrência e a dívida técnica.

**Placar do grupo após esta decisão:** 🔥 8 · 💰 R$ 35 mil · 🧹 1

---

## RODADA 2: A injeção que derreteu o banco (21h02)

**Tipo:** rodada · **Voto:** D

**Justificativa:**

Utilizar uma consulta derivada do Spring Data JPA, como `findByTitular`, evitando a montagem manual de consultas SQL e aproveitando a parametrização oferecida pelo framework. Embora demande mais tempo de implementação, essa alternativa facilita a manutenção e reduz o risco de falhas na construção das consultas.

**Placar do grupo após esta decisão:** 🔥 8 · 💰 R$ 35 mil · 🧹 0

---

## RODADA 3: A noite das contas gêmeas (22h50)

**Tipo:** rodada · **Voto:** A

**Justificativa:**

Corrigir o método `getInstance()` para reutilizar a mesma instância do gerador de contas, evitando que o contador seja reiniciado e produza números duplicados. Essa solução é rápida e preserva a geração sequencial exigida pelo sistema, embora seja importante garantir o comportamento correto em acessos concorrentes.

**Placar do grupo após esta decisão:** 🔥 9 · 💰 R$ 40 mil · 🧹 0

---

## RODADA 4: O comprovante que aponta pra pessoa errada (00h47)

**Tipo:** rodada · **Voto:** B

**Justificativa:**

Corrigir a ordem dos campos de origem e destino na gravação do extrato e reprocessar os registros antigos para restaurar a integridade dos dados. Embora a solução leve cerca de duas horas e gere maior prejuízo imediato, ela garante que os comprovantes antigos e novos respeitem o contrato do banco e reduz a dívida técnica.

**Placar do grupo após esta decisão:** 🔥 10 · 💰 R$ 65 mil · 🧹 -1

---

## RODADA 5: A fábrica que devolve null (01h30)

**Tipo:** rodada · **Voto:** A

**Justificativa:**

Alterar a `ContaFactory` para lançar uma exceção clara quando receber um tipo de conta inválido, evitando que retorne `null` e cause um erro genérico posteriormente no repositório. Embora a decisão reduza a reputação em 1 ponto, ela identifica a falha na origem, facilita o diagnóstico e evita aumentar a dívida técnica.

**Placar do grupo após esta decisão:** 🔥 9 · 💰 R$ 65 mil · 🧹 -1

---

## EVENTO RELÂMPAGO C: O cadastro com metade dos campos (02h20)

**Tipo:** relâmpago · **Voto:** B

**Justificativa:**

Adotar o padrão Builder para concentrar valores padrão e validações na construção do objeto, permitindo o cadastro com os campos disponíveis sem criar mais construtores sobrecarregados. Essa abordagem atende à necessidade da landing page sem aumentar a dívida técnica, ao contrário da criação de novos construtores ou da propagação de valores `null`.

**Placar do grupo após esta decisão:** 🔥 9 · 💰 R$ 65 mil · 🧹 -1

---

## RODADA 6: O teste vermelho às 03h45 (a janela fecha às 04h)

**Tipo:** rodada · **Voto:** B

**Justificativa:**

Investigar a causa da taxa TED incorreta e corrigir o serviço, mantendo o teste que valida o contrato de R$ 5,00. Embora a equipe perca a janela de deploy e tenha prejuízo imediato, evita mascarar a falha desabilitando ou alterando o teste e reduz a dívida técnica.

**Placar do grupo após esta decisão:** 🔥 8 · 💰 R$ 95 mil · 🧹 -2

---

## RODADA 7: Go/No-Go da Black Friday (06h15)

**Tipo:** rodada · **Voto:** B

**Justificativa:**

Corrigir o problema identificado no TransferenciaService e revisar a taxa TED antes de abrir a campanha. Embora a abertura com duas horas de atraso gere prejuízo financeiro e reduza o aproveitamento inicial da Black Friday, a decisão prioriza a confiabilidade das transações e evita expor os clientes a cobranças indevidas.

**Placar do grupo após esta decisão:** 🔥 9 · 💰 R$ 195 mil · 🧹 -3

---

## 🔎 O caminho do MEU grupo (preencher na 3ª aula, quando o mapa for revelado)

Uma linha por decisão registrada acima (usem os títulos ditados em aula):

| # | Decisão | Nossa letra | Consequência que ELA teria tido |
|---|---|---|---|
| 1 | O saldo que virou negativo | C | 🔥 +1 · 💰 +R$ 35 mil · 🧹 -1 |
| 2 | A injeção que derreteu o banco | D | 🔥 0 · 💰 R$ 0 mil · 🧹 -1 |
| 3 | A noite das contas gêmeas | A | 🔥 +1 · 💰 +R$ 5 mil · 🧹 0 |
| 4 | O comprovante que aponta para a pessoa errada | B | 🔥 +1 · 💰 +R$ 25 mil · 🧹 -1 |
| 5 | A fábrica que devolve null | A | 🔥 -1 · 💰 R$ 0 mil · 🧹 0 |
| 6 | EVENTO RELÂMPAGO C: O cadastro com metade dos campos | B | Sem alteração nos indicadores |
| 7 | O teste vermelho às 03h45 (a janela fecha às 04h) | B | 🔥 -1 · 💰 +R$ 30 mil · 🧹 -1 |
| 8 | Go/No-Go da Black Friday | B | 🔥 +1 · 💰 +R$ 100 mil · 🧹 -1 |

*(adicionem linhas conforme as decisões da madrugada)*

---

# 📋 PÓS-MORTEM · relatório de incidente (montar em sala na 3ª aula)

> Rascunho em aula e commit final `pos-mortem:` até 23h59 do dia da 3ª aula.

## 1. Linha do tempo da madrugada

Nas primeiras rodadas, o grupo tomou decisões para corrigir problemas financeiros e técnicos do FiapBank, considerando os impactos sobre o saldo, a estabilidade do sistema e a dívida técnica.

Na Rodada 2, o grupo lidou com um problema de parametrização da consulta SQL/JDBC. Na Rodada 5, enfrentou uma falha relacionada à Factory, em que um tipo inválido retornava null, causando uma exceção.

Na Rodada 6, o grupo decidiu investigar a causa da taxa TED incorreta e corrigir o serviço, mantendo o teste que validava o contrato de R$ 5,00. Na Rodada 7, decidiu priorizar a confiabilidade das transações e revisar a taxa TED antes de abrir a campanha de Black Friday, mesmo com o atraso de duas horas.

## 2. Causa raiz de 2 incidentes (aula + mecanismo técnico)

**Incidente 1: R2 — A injeção que derreteu o banco**

Aula/mecanismo: Aula 12 — Conectando ao Banco Oracle (JDBC). O problema está relacionado à construção insegura de consultas SQL, que pode permitir SQL Injection. O uso de consultas parametrizadas ajuda a impedir que entradas do usuário sejam interpretadas como parte do comando SQL, aumentando a segurança e a integridade dos dados.

**Incidente 2: R5 — A fábrica que devolve null**

Aula/mecanismo: Aula 14 — A Fábrica de ObjSetos (Padrões Criacionais). O problema ocorre quando a ContaFactory recebe um tipo inválido e retorna null. Isso transfere o erro para o ponto em que o objeto é utilizado, podendo provocar uma exceção e dificultar o diagnóstico. Uma alternativa é lançar uma exceção clara quando o tipo não for suportado, tornando a falha explícita.

## 3. O que faríamos diferente (2 rodadas + por quê)

**Rodada 1:** Investigar melhor as causas do problema antes de decidir, avaliando os impactos técnicos e financeiros para evitar que uma correção rápida gere novos riscos.

**Rodada 6:** Manter a validação do contrato de R$ 5,00 e priorizar a correção da causa raiz da taxa TED, evitando mascarar a falha e aumentar a dívida técnica.

## 4. A maior lição da equipe

Aprendemos que decisões técnicas precisam equilibrar urgência operacional, impacto financeiro, integridade dos dados e confiança dos clientes. Corrigir a causa raiz e preservar os testes é essencial para manter a confiabilidade do sistema, mesmo quando isso exige atrasar uma entrega ou campanha.
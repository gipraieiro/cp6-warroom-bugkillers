# Justificativa de ausência — Aula 1 (13/10)

**Grupo:** BugKillers  
**Turma:** 2CCPO  
**Integrantes:** Gabrielly Lorentz (565806), Giovanna Praieiro (565681), Julia Aparicio (563623), Heitor Barbosa (563078), Maria Eduarda de Oliveira (565386) e Nicolle Calasans (564381).

Este documento reúne as decisões hipotéticas que o grupo teria tomado caso estivesse presente na primeira aula. Após conversar em conjunto, consideramos as alternativas abaixo mais adequadas diante dos incidentes apresentados e dos impactos esperados.

## RODADA 1 — O saldo que virou negativo

**Opção escolhida: C**

Escolheríamos corrigir a regra de negócio que impede o saldo de ficar negativo e acrescentar um teste com `assertThrows` para confirmar que uma transferência sem saldo suficiente é recusada. Embora essa solução demande mais trabalho do que um ajuste rápido, ela trata a causa do problema e ajuda a evitar que o incidente volte a acontecer. Consideramos que preservar a integridade do saldo e testar o comportamento esperado é mais importante do que apenas corrigir o sintoma com rapidez.

## RODADA 2 — A injeção que derreteu o banco

**Opção escolhida: D**

Escolheríamos utilizar uma consulta derivada do Spring Data JPA, como `findByTitular`, em vez de montar consultas SQL manualmente. Essa abordagem aproveita os recursos do framework e reduz a necessidade de construir consultas por concatenação, diminuindo o risco de falhas e de injeção. Mesmo que a implementação possa exigir mais tempo, ela favorece a segurança e a manutenção do sistema.

## RODADA 3 — A noite das contas gêmeas

**Opção escolhida: A**

Escolheríamos corrigir o método `getInstance()` para reutilizar a mesma instância do gerador de contas, evitando que o contador fosse reiniciado e produzisse números duplicados. A alternativa atende à exigência de gerar números sequenciais e únicos e resolve o problema na origem. Também consideraríamos importante validar o comportamento em situações de acesso concorrente, para que a correção continue confiável quando houver várias operações ao mesmo tempo.

## Resumo das escolhas

| Rodada | Opção |
|---|---|
| R1 — O saldo que virou negativo | C |
| R2 — A injeção que derreteu o banco | D |
| R3 — A noite das contas gêmeas | A |

**Observação:** as escolhas e justificativas são hipotéticas e foram consolidadas pelo grupo após conversa entre os integrantes, como reposição da aula em que não pudemos estar presentes.

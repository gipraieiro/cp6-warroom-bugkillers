# Justificativa de ausência — Aula 2 (20/10)

**Grupo:** BugKillers  
**Turma:** 2CCPO  
**Integrantes:** Gabrielly Lorentz (565806), Giovanna Praieiro (565681), Julia Aparicio (563623), Heitor Barbosa (563078), Maria Eduarda de Oliveira (565386) e Nicolle Calasans (564381).

Este documento reúne as decisões hipotéticas que o grupo teria tomado caso estivesse presente na segunda aula. As escolhas foram discutidas em conjunto e justificadas considerando o funcionamento esperado do sistema, os riscos para os clientes e os impactos técnicos e financeiros.

## RODADA 4 — O comprovante que aponta para a pessoa errada

**Opção escolhida: B**

Escolheríamos corrigir a ordem dos campos de origem e destino na gravação do extrato e reprocessar os registros antigos afetados. A decisão exige mais tempo e gera um custo imediato maior, mas é importante que os comprovantes representem corretamente quem enviou e quem recebeu a transferência. Preferimos corrigir os dados e a causa do erro em vez de manter registros incorretos, pois isso comprometeria a integridade das informações e a confiança dos clientes.

## RODADA 5 — A fábrica que devolve null

**Opção escolhida: A**

Escolheríamos alterar a `ContaFactory` para lançar uma exceção clara quando receber um tipo de conta inválido, em vez de retornar `null`. Assim, a falha seria identificada no ponto em que ocorre, facilitando o diagnóstico e evitando que um erro genérico apareça posteriormente no repositório. Apesar do impacto negativo na reputação indicado para essa escolha, consideramos melhor tornar o comportamento explícito e previsível do que deixar a falha se propagar.

## EVENTO RELÂMPAGO C — O cadastro com metade dos campos

**Opção escolhida: B**

Escolheríamos adotar o padrão Builder para concentrar a construção do objeto, os valores padrão e as validações em um único lugar. Isso permite lidar com cadastros que chegam com apenas parte dos campos sem criar vários construtores sobrecarregados nem espalhar valores `null` pelo código. Consideramos que o Builder organiza melhor a criação dos objetos e facilita a manutenção, sem acrescentar dívida técnica desnecessária.

## RODADA 6 — O teste vermelho às 03h45 (a janela fecha às 04h)

**Opção escolhida: B**

Escolheríamos investigar a causa da taxa TED incorreta e corrigir o serviço, mantendo o teste que verifica o contrato de taxa fixa de R$ 5,00. Embora isso signifique perder a janela de deploy e assumir um prejuízo imediato, desabilitar ou alterar o teste apenas esconderia o problema e deixaria o sistema sujeito a cobrar valores incorretos. Para o grupo, é preferível corrigir a causa e preservar a validação do comportamento esperado, reduzindo o risco de recorrência.

## Resumo das escolhas

| Etapa | Opção |
|---|---|
| R4 — O comprovante que aponta para a pessoa errada | B |
| R5 — A fábrica que devolve null | A |
| Evento Relâmpago C — O cadastro com metade dos campos | B |
| R6 — O teste vermelho às 03h45 | B |

**Observação:** as escolhas e justificativas são hipotéticas e foram consolidadas pelo grupo após conversa entre os integrantes, como reposição da aula em que não pudemos estar presentes.

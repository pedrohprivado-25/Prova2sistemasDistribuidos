# P2 - SISTEMAS DISTRIBUÍDOS 

**ALUNO:** PEDRO HENRIQUE PRIVADO ALVES

**RA:** 409676312035

**PROFESSORA:** Simone Tatiane do Canto 

**DATA:** 18/11/2026

## 1.Situação-Problema

Uma empresa de comércio eletrônico utiliza um sistema para controlar o processamento de
pedidos.

O sistema possui um servidor principal e uma réplica responsável por manter uma cópia das
informações.

a equipe de TI identificou três problemas:
1. O sistema está permitindo mais processamentos simultâneos do que o limite  estabelecido pela empresa.
2. O estoque da réplica não está sendo atualizado corretamente.
3. Quando o servidor principal fica indisponível, o sistema não consegue utilizar a réplica.

## 2.Primeira Excecução

=== CENTRAL DE PEDIDOS ===

Limite simultâneo: 3

Estoque inicial: 10

Pedidos confirmados: [101, 102, 103]

Estoque principal: 7

Réplica antes: 10

Réplica depois: 10

Consulta após falha: REPLICA 10


### Evidência 1 - Primeira Execução
![Evidência 1](evidencias/evidencia1.png)

## 3.Probelma A - Concorrência

Qual Problema Foi Identificado?

O problema identificado foi que o sistema estava permitindo até três processamentos simultâneos, enquanto o limite estabelecido pela empresa era de apenas dois pedidos por vez.

Qual Alteração foi realizada?

Alterei o valor de MAXIMO_SIMULTANEO = 3 para MAXIMO_SIMULTANEO = 2. Essa alteração ajusta o semáforo para permitir que, no máximo, duas threads processem pedidos simultaneamente, respeitando o limite definido pela empresa.

### Evidência 2 - Concorrência
![Evidência 2](evidencias/evidencia2.png)

## 4.Probelma B - Replicação e Consistência

Qual Problema Foi Indentificado?

O problema identificado estava na função sincronizar(). A réplica estava recebendo o próprio valor de estoque novamente, ou seja, não estava recebendo as atualizações feitas no servidor principal. Por isso, mesmo depois do processamento dos pedidos, o estoque da réplica continuava com 10 unidades.

Qual alteração foi realizada?

Alterei a linha replica["estoque"] = replica["estoque"] para replica["estoque"] = principal["estoque"]. Dessa forma, a réplica passa a receber o estoque atualizado do servidor principal.

Depois do processamento dos três pedidos, o estoque principal passou de 10 para 7 unidades. Após a sincronização, o estoque da réplica também passou de 10 para 7 unidades.

### Evidência 3 - Sincronização
![Evidência 3](evidencias/evidencia3.png)


## 5.Probelma C - Tolerância a Falhas

Qual Problema Foi Indentificado?

O problema identificado estava na função consultar_estoque(). A segunda condição verificava novamente se o servidor principal estava online, em vez de verificar a disponibilidade da réplica. Com isso, quando o servidor principal ficava indisponível, o sistema não conseguia utilizar corretamente a réplica.

Qual alteração foi realizada?

lterei a segunda condição de if principal["online"] para if replica["online"]. Dessa forma, quando o servidor principal fica indisponível, o programa verifica se a réplica está online e utiliza o estoque dela para continuar respondendo às consultas. Essa alteração simula um processo de failover, permitindo que o sistema utilize a réplica quando o servidor principal apresenta uma falha.

### Evidência 4 - Tolerância a falhas
![Evidência 4](evidencias/evidencia4.png)

## 6.VALIDAÇÃO DO PROGRAMA

Após realizar as três correções, executei o programa novamente e os resultados foram apresentados conforme o esperado. O limite de processamento simultâneo passou a ser 2, o estoque principal foi atualizado de 10 para 7 unidades e a réplica recebeu a mesma atualização após a sincronização. Também foi possível verificar que, quando o servidor principal ficou indisponível, o sistema conseguiu utilizar a réplica para consultar o estoque, apresentando o resultado REPLICA 7.

### Evidência 5 - Execução Completa 
![Evidência 5](evidencias/evidencia5.png)



## 7.Análise Técnica

## Questão 1 — Qual é a função do semáforo no processamento dos pedidos?

O semáforo serve para controlar quantas threads podem executar uma tarefa ao mesmo tempo. Nesse programa, ele permite que apenas duas tarefas sejam processadas simultaneamente, evitando que muitas threads acessem a mesma região de execução de uma vez.

## Questão 2 — Por que é necessário sincronizar os dados entre o servidor principal e a réplica?

A sincronização é importante para manter os dois servidores com as mesmas informações. Assim, quando o servidor principal atualiza o estoque após o processamento dos pedidos, a réplica também recebe esses dados, evitando diferenças nas informações armazenadas.

## Questão 3 — O que significa tolerância a falhas e como ela aparece neste programa?

A tolerância a falhas permite que um sistema continue funcionando mesmo quando alguma parte apresenta problemas. No programa, isso acontece quando o servidor principal fica indisponível e a réplica assume o atendimento das consultas, permitindo que o serviço continue funcionando.

## Questão 4 — Para que serve o Lock no programa e como o controle de acesso aos recursos ajuda a prevenir problemas de concorrência?

O Lock serve para controlar o acesso aos dados compartilhados, permitindo que apenas uma thread por vez realize determinada operação. No programa, ele protege as alterações no estoque e na lista de pedidos confirmados, evitando que duas threads modifiquem essas informações ao mesmo tempo e causem erros. Também é importante organizar o acesso aos recursos para evitar problemas como os deadlocks.

## Questão 5 — A sincronização do estoque deve acontecer antes ou depois do processamento dos pedidos? Explique a relação de causalidade.

A sincronização deve acontecer depois do processamento dos pedidos, pois primeiro o servidor principal precisa atualizar o estoque com as novas informações. Depois disso, os dados atualizados são enviados para a réplica. Dessa forma, os dois servidores permanecem com informações corretas e seguem a ordem em que as operações precisam acontecer.

## 8.Conclusão

Com a realização desta atividade, consegui entender melhor como funciona o processamento de pedidos em um sistema distribuído. Durante as correções, aprendi a controlar a quantidade de tarefas simultâneas, sincronizar os dados entre o servidor principal e a réplica e utilizar a réplica quando o servidor principal apresenta uma falha. Também pude colocar em prática os conteúdos estudados em aula e perceber a importância dessas funções para o funcionamento correto de um sistema.

## 9.Entrega 

**ALUNO:** PEDRO HENRIQUE PRIVADO ALVES

**Repositorio:**[https://github.com/pedrohprivado-25/Prova2sistemasDistribuidos]

**DATA:** 18/11/2026

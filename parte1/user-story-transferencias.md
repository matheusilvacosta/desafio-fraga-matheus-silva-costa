# US1 — Transferência entre contas próprias

## Objetivo

Validar se o cliente consegue transferir dinheiro entre duas contas que pertencem a ele, garantindo que a transferência siga as regras informadas e que os saldos e históricos das duas contas sejam atualizados corretamente.

## Critérios de aceitação

### CA01 — Contas devem ser do mesmo titular

A transferência apenas deve ser permitida apenas entre as contas pertencentes ao mesmo titular.

### CA02 — Valores devem ser maiores que zero

O valor das transferências devem ser maiores do que zero, com no máximo duas casas decimais.

### CA03 — Atualização do saldo das contas

Ao final da operação, o saldo e o histórico de transações de ambas as contas devem refletir a transferência.

## Test Cases

```gherkin
# language: pt

Funcionalidade: Transferência entre contas próprias
  Como cliente do banco digital
  Quero transferir dinheiro entre duas contas da minha titularidade
  Para organizar minhas finanças

  Contexto:
    Dado que o cliente está na funcionalidade de transferência entre contas


  @CA01 @positivo
  Cenário: Realizar transferência entre contas do mesmo titular
    Dado que a conta de origem e a conta de destino pertencem ao mesmo titular
    Quando o cliente realiza uma transferência com um valor válido
    Então a transferência deve ser permitida


  @CA01 @negativo
  Cenário: Impedir transferência entre contas de titulares diferentes
    Dado que a conta de origem e a conta de destino pertencem a titulares diferentes
    Quando o cliente tenta realizar a transferência
    Então a transferência não deve ser permitida


  @CA02 @positivo @fronteira
  Esquema do Cenário: Realizar transferência com valor válido
    Dado que a conta de origem e a conta de destino pertencem ao mesmo titular
    Quando o cliente informa o valor "<valor>" para transferência
    E confirma a operação
    Então a transferência deve ser permitida

    Exemplos:
      | valor    |
      | 0,01     |
      | 10,99    |


  @CA02 @negativo @fronteira
  Esquema do Cenário: Impedir transferência com valor inválido
    Dado que a conta de origem e a conta de destino pertencem ao mesmo titular
    Quando o cliente informa o valor "<valor>" para transferência
    E tenta confirmar a operação
    Então a transferência não deve ser permitida

    Exemplos:
      | valor     |
      | 0         |
      | -0,01     |
      | 10,999    |


  @CA03 @saldo
  Cenário: Atualizar os saldos das duas contas após a transferência
    Dado que a conta de origem e a conta de destino pertencem ao mesmo titular
    Quando o cliente realiza uma transferência com um valor válido
    Então o saldo da conta de origem deve refletir a transferência
    E o saldo da conta de destino deve refletir a transferência


  @CA03 @historico
  Cenário: Atualizar os históricos das duas contas após a transferência
    Dado que a conta de origem e a conta de destino pertencem ao mesmo titular
    Quando o cliente realiza uma transferência com um valor válido
    Então o histórico da conta de origem deve refletir a transferência
    E o histórico da conta de destino deve refletir a transferência


  @CA03 @integridade
  Cenário: Manter consistência entre saldo e histórico após a transferência
    Dado que a conta de origem e a conta de destino pertencem ao mesmo titular
    Quando o cliente realiza uma transferência com um valor válido
    Então os saldos das duas contas devem refletir a transferência
    E os históricos das duas contas devem refletir a mesma transferência
```

## Dúvidas e suposições

### Dúvidas

**D01 — Cliente sem saldo**  
O que deve acontecer quando um usuário tentar realizar uma transferência com o saldo maior do que ele possui em conta?

**D02 — Transferência para a mesma conta**  
O usuário pode transferir esse dinheiro para a mesma conta ou apenas uma conta de mesma titularidade?

**D03 — Limites das transferências não definidos**  
Deve existir algum tipo de valor máximo ou mínimo para as transferências?

**D04 — Tratamento das casas decimais**  
Dentro dos requisitos não há um critério sobre como tratar as casas decimais adjacentes, minha opinião é que o campo não permita o usuário fazer esse tipo de preenchimento (Com casas decimais além do especificado pelo requisito).

**D05 — Deve-se haver uma forma de assegurar que a transferência seja concluída**  
Há algum tratamento para caso o usuário inicie a transação e haja algum tipo de desconexão? O dinheiro seria debitado e se perdido ou teríamos algum tipo de aviso indicando que o usuário deve tentar novamente mais tarde?

**D06 — Definiçoes sobre o histórico**  
Definir com o time qual tipo de informação será salva no histórico, como ID, status, data.


### Suposições adotadas

Supondo que estamos refinando nossa user story, utilizei apenas os critérios de aceite já indicados no documento, num cenário real o time analisaria esses pontos de dúvida, com intuíto de trazer uma US mais interada, vendo o que faz sentido atuar ou não.
Pontos não definidos nessa versão, como: saldo insuficiente, limite máximo/ mínimo, estado das contas ou falhas parciais, não foram atendidos.
Para os testes de valor, foi considerado que valores com mais de duas casas decimais são inválidos, além disso técnicas de partição de equivalência e valor limite foram utilizados.

## Priorização dos testes

Caso apenas 30% dos testes consigam ser realizados, deveríamos executar os seguintes testes respectivamente:

### Realizar transferência entre contas do mesmo titular

Este é o fluxo principal da funcionalidade. Caso ele não funcione corretamente, o cliente não consegue utilizar a transferência entre suas próprias contas. Além disso todos os critérios de aceite estabelecidos são atendidos, sendo um bom ponto de partida para iniciarmos um futuro teste de regressão.

### Impedir transferência entre contas de titulares diferentes

Valida uma regra de negócio explícita e crítica. Permitir uma transferência fora da mesma titularidade significaria aceitar uma operação que não deveria ser permitida pela funcionalidade.

### Consistência entre saldo e histórico após a transferência

Valida se a movimentação financeira foi refletida corretamente nas duas contas. Uma inconsistência entre saldo e histórico pode indicar que a transferência foi processada parcialmente ou registrada incorretamente.

### Impedir transferência com valor inválido

Verifica corretamente se o programa está tratando dados tratados como inválidos pela regra, impedindo transferências negativas, por exemplo.

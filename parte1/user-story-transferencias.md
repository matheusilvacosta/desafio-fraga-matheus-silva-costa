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
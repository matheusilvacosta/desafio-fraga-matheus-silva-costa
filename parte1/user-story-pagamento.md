# Pagamento de contas

## Objetivo

Validar se o cliente consegue realizar o pagamento de contas utilizando o saldo de sua conta, garantindo que o pagamento siga as regras definidas e que operações inválidas sejam impedidas.

## Critérios de aceitação

### CA01 — Pagamento através do código de barras

O cliente deve informar o código de barras da conta para realizar o pagamento.

### CA02 — Pagamentos apenas em dias úteis

O pagamento deve ser permitido apenas quando realizado em dias úteis.

### CA03 — Pagamentos inválidos devem ser impedidos

O sistema deve impedir a realização de pagamentos considerados inválidos.

## Test Cases

```gherkin
# language: pt

Funcionalidade: Pagamento de contas
  Como cliente do banco digital
  Quero pagar contas utilizando o saldo da minha conta
  Para não precisar ir a um banco físico

  Contexto:
    Dado que o cliente está na funcionalidade de pagamento de contas


  @CA01 @positivo
  Cenário: Realizar pagamento informando um código de barras
    Dado que o cliente informa um código de barras
    Quando solicita o pagamento
    Então o sistema deve processar a solicitação de pagamento


  @CA02 @positivo
  Esquema do Cenário: Permitir pagamento em dia útil
    Dado que o cliente informa um código de barras
    E a data da operação corresponde a "<dia>"
    Quando solicita o pagamento
    Então o pagamento deve ser permitido

    Exemplos:
      | dia           |
      | segunda-feira |
      | terça-feira   |
      | quarta-feira  |
      | quinta-feira  |
      | sexta-feira   |


  @CA02 @negativo
  Esquema do Cenário: Impedir pagamento em dia não útil
    Dado que o cliente informa um código de barras
    E a data da operação corresponde a "<dia>"
    Quando solicita o pagamento
    Então o pagamento não deve ser permitido

    Exemplos:
      | dia     |
      | sábado  |
      | domingo |
      | feriado |


  @CA03 @negativo
  Cenário: Impedir pagamento inválido
    Dado que o cliente informa dados que resultam em um pagamento inválido
    Quando solicita o pagamento
    Então o sistema deve impedir a realização do pagamento
```
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

## Dúvidas e suposições

### Dúvidas

**D01 — O que caracteriza um pagamento inválido?**  
O requisito informa que pagamentos inválidos devem ser impedidos, mas não define quais situações tornam um pagamento inválido, seriam boletos vencidos? Sem saldo o suficiente? Pagamento iniciado fora dos dias úteis?

**D02 — Validação do código de barras**  
Não está definido quais validações devem ser aplicadas ao código de barras informado, como quantidade de dígitos, formato esperado ou validade do código.

**D03 — Pagamento de conta já paga**  
Não está definido como o sistema deve se comportar caso o cliente tente pagar uma conta que já foi processada anteriormente, minha sugestão é que seja barrada a operação assim que verificado que o boleto já foi pago.

**D04 — Tratamento de feriados**  
O requisito informa que pagamentos só podem ser realizados em dias úteis, mas não especifica como feriados nacionais, estaduais ou municipais devem ser tratados, por definição feriados não são considerados dias úteis, então criei um campo geral para feriado, nos casos de teste.

**D05 — Horário limite para pagamentos**  
Não está definido se existe algum horário limite para realizar pagamentos em dias úteis.

**D06 — Pagamento realizado em dia não útil**  
Para melhor experiência do nosso cliente, devemos pensar em uma forma de agendar esse pagamento para o próximo dia útil e, caso o cliente tenha saldo nesse respectivo dia, o pagamento é realizado.

**D07 — Consistência de saldo**  
O requisito não informa explicitamente se o saldo da conta deve ser atualizado imediatamente após a conclusão do pagamento.

**D08 — Consistência de histórico**  
Não está definido se o pagamento deve gerar um registro no histórico da conta e quais informações devem ser armazenadas, como valor, data, código da operação ou status.

### Suposições adotadas

Para os testes definidos nesta etapa, foram utilizados apenas os critérios de aceite explicitamente descritos na user story.
Não foram assumidos comportamentos específicos para contas vencidas, contas já pagas, saldo insuficiente, feriados, horários limite ou agendamento, pois esses pontos não estão definidos no requisito.
Para os testes relacionados aos dias úteis, foram considerados segunda-feira a sexta-feira como cenários permitidos e sábado e domingo como cenários não permitidos. O tratamento de feriados permanece como uma dúvida, mas ainda identificada de forma geral nos testes.
Como o requisito não define o que caracteriza um pagamento inválido, o cenário relacionado ao CA03 foi mantido de forma genérica até que os critérios de invalidez sejam esclarecidos.

## Priorização dos testes

Caso apenas 30% dos testes possam ser executados, os seguintes cenários devem ser priorizados:

### Realizar pagamento informando um código de barras

Este é o fluxo principal da funcionalidade. Caso ele não funcione corretamente, o cliente não consegue realizar o pagamento de contas pelo sistema.
Esse cenário também serve como ponto inicial para validar se a funcionalidade básica está disponível antes da execução dos demais testes.

### Permitir pagamento em dia útil

Valida uma das regras de negócio explícitas da user story. Como o requisito determina que pagamentos só podem ser realizados em dias úteis, é importante garantir que o sistema permita a operação dentro do período esperado.

### Impedir pagamento em dia não útil

Valida o comportamento oposto da mesma regra de negócio, garantindo que o sistema não permita o pagamento quando a operação for realizada fora de um dia útil.

### Impedir pagamento inválido

Valida uma regra diretamente definida no requisito: pagamentos inválidos não devem ser processados.
Como o requisito ainda não define o que caracteriza um pagamento inválido, este cenário permanece genérico até que essa regra seja refinada com o time.

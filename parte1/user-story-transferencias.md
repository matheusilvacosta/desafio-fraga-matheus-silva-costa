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


# Parte 3 — Testes de API

## Objetivo

Validar os principais comportamentos da API Restful Booker, cobrindo autenticação, fluxo CRUD de reservas, validações de contrato e cenários negativos.

## Ferramentas utilizadas

- Postman
- Newman

## Arquivos

- `Desafio Fraga.postman_collection.json` — Collection com os testes da API.
- `Desafio QA.postman_environment.json` — Environment com as variáveis utilizadas pela suíte.

## Variáveis de ambiente

A suíte utiliza as seguintes variáveis:

- `baseUrl` — URL base da Restful Booker. Antes da execução, preencher com `https://restful-booker.herokuapp.com`.
- `token` — preenchido dinamicamente após a autenticação.
- `bookingId` — preenchido dinamicamente após a criação de uma reserva.

## Estrutura da suíte

### Auth

- `Auth` — gera um token de autenticação, valida o status `200`, verifica se o token existe e salva o valor em `{{token}}`.

### CRUD

A execução foi organizada na seguinte ordem para permitir o encadeamento das requisições:

1. `Create Booking` — cria uma reserva e salva o identificador em `{{bookingId}}`.
2. `Get Booking` — consulta a reserva criada e valida os dados originais.
3. `Update Booking` — atualiza os dados da reserva utilizando `{{token}}`.
4. `Get Updated Booking` — confirma que os dados alterados foram persistidos.
5. `Delete Booking` — exclui a reserva utilizando `{{token}}`.
6. `Get Deleted Booking` — valida que a reserva excluída não está mais disponível, esperando `404`.

### Contract Validation

- `Create Booking Missing Field` — valida o comportamento da API quando um campo é omitido.
- `Create Booking Wrong Type` — envia JSON válido com tipo de dado incorreto em `totalprice` e valida o comportamento retornado pela API.

### Negative Scenarios

Foram incluídos cenários para operações protegidas sem autenticação válida:

- `Update Booking - Without Authentication`
- `Update Booking - Wrong Token`
- `Delete Booking Without Token`
- `Delete Booking Wrong Token`

Esses cenários validam resposta `403`.

Também foram incluídos métodos não suportados:

- `PATCH /auth` — retorno observado e validado: `404`.
- `POST /booking/{{bookingId}}` — retorno observado e validado: `404`.

## Encadeamento das requisições

A suíte foi construída para reutilizar valores gerados durante a própria execução:

```text
Auth
  ↓
token
  ↓
Create Booking
  ↓
bookingId
  ↓
Get Booking
  ↓
Update Booking
  ↓
Get Updated Booking
  ↓
Delete Booking
  ↓
Get Deleted Booking
```

Isso evita a necessidade de copiar manualmente token e ID entre as requisições.

## Como executar no Postman

1. Importar `Desafio Fraga.postman_collection.json`.
2. Importar `Desafio QA.postman_environment.json`.
3. No Environment, preencher `baseUrl` com:

```text
https://restful-booker.herokuapp.com
```

4. Selecionar o Environment importado.
5. Executar primeiro a pasta `Auth`.
6. Executar a pasta `CRUD` na ordem configurada.
7. Executar as pastas de validação de contrato e cenários negativos separadamente.

## Como executar com Newman

Com Node.js e Newman instalados:

```bash
npm install -g newman
```

Depois execute:

```bash
newman run "Desafio Fraga.postman_collection.json" -e "Desafio QA.postman_environment.json" --env-var baseUrl=https://restful-booker.herokuapp.com
```

## Decisões adotadas

- `token` e `bookingId` não ficam preenchidos no arquivo de Environment porque são gerados dinamicamente durante a execução.
- O fluxo CRUD foi ordenado para garantir que cada etapa utilize o estado criado pela etapa anterior.
- Nos cenários de método HTTP não suportado, os asserts utilizam os códigos realmente observados durante a execução da API.
- O teste de tipo inválido utiliza JSON sintaticamente válido para diferenciar erro de tipo de erro de formatação do payload.

## Limitações e pontos que seriam ampliados com mais tempo

A versão atual da suíte possui cobertura de campo ausente e tipo inválido na validação de contrato. Com mais tempo, seriam adicionados cenários específicos para payload incompleto e JSON malformado, além de ampliar a cobertura de recursos inexistentes e outros métodos HTTP.

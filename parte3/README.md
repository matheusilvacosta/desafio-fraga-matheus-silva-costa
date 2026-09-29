# Parte 3 — Testes de API

## Objetivo

Validar os principais comportamentos da API Restful Booker, incluindo
autenticação, fluxo CRUD, contrato das requisições e cenários negativos.

## Ferramentas

- Postman

## Estratégia

A suíte foi organizada em quatro grupos:

1. Autenticação
2. CRUD de reservas
3. Validação de contrato
4. Cenários negativos e códigos HTTP

O fluxo principal utiliza variáveis de ambiente para permitir o
encadeamento das requisições, reutilizando o token de autenticação e
o identificador da reserva criada durante a execução.

## Variáveis de ambiente

A suíte utiliza variáveis de ambiente para permitir o encadeamento das
requisições.

- `baseUrl`: endereço base da Restful Booker.
- `token`: token de autenticação obtido através da request `Generate Token`.
- `bookingId`: identificador da reserva criada durante a execução.

O token e o bookingId são preenchidos dinamicamente durante a execução
da Collection.
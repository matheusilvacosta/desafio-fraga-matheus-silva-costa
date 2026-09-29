# Parte 2 — Teste Exploratório

## Charter da sessão
Investigar o fluxo completo de compra da aplicação SauceDemo, passando
por catálogo, carrinho e checkout, com o objetivo de identificar problemas
funcionais, inconsistências de dados e problemas de usabilidade que possam
impactar um cliente real.

## Ambiente
- Aplicação: SauceDemo (https://www.saucedemo.com/)
- Usuário: standard_user
- Sistema operacional: Windows 11
- Navegador: Firefox 156.0.1
- Data: 29/09/2026

## Estratégia utilizada
A sessão foi iniciada com uma exploração livre da aplicação, sem um objetivo
específico além de conhecer sua estrutura, funcionalidades e comportamento.

Após esse reconhecimento inicial, a exploração passou a ser direcionada para:

- navegação e catálogo de produtos;
- links e elementos disponíveis;
- preenchimento e tratamento de dados;
- carrinho de compras;
- checkout;
- consistência das informações apresentadas ao usuário.

Durante parte da exploração, o Inspetor (Firefox) estava aberto, para auxiliar a investigação.

## Distribuição do tempo
### 10 minutos — Exploração livre
Navegação geral pela aplicação sem foco em um cenário específico, buscando
entender as funcionalidades disponíveis e o comportamento do produto.

### 15 minutos — Navegação e catálogo
Verificação dos links, produtos catalogados e diferentes formas de apresentação
do catálogo, utilizando também o Chrome DevTools como apoio durante a exploração.

### 10 minutos — Preenchimento e tratamento de dados
Exploração dos campos utilizados durante o fluxo de compra, verificando como
a aplicação recebe e trata os valores informados pelo usuário.

### 10 minutos — Carrinho e fluxo de compra
Adição de itens ao carrinho, navegação pelo fluxo de compra e verificação das
informações apresentadas nas etapas de carrinho e checkout.

### 15 minutos — Reproduzir os bugs encontrados, passar anotações a limpo e entender quais bugs poderiam ser melhorias e vice-versa
Os comportamentos encontrados durante a exploração foram reproduzidos novamente
para confirmar sua consistência, impacto e passos necessários para reprodução.


## Observações da sessão
### OBS01 — Dynamic Catalog — Spinner
Foi observado que o comportamento do Dynamic Catalog — Spinner apresenta os
produtos de forma semelhante à opção All Items.
Não foi possível determinar durante a sessão qual diferença funcional é esperada
entre as duas opções.


### OBS02 — Dynamic Catalog — Slider
A apresentação dos produtos no modo Slider poderia fornecer indicadores mais
claros de navegação ao usuário.
O tempo de permanência de cada produto também pode dificultar a leitura das
informações antes da troca automática.

Oportunidade de melhoria.


### OBS03 — Expiração da sessão
Durante a exploração foi observado que a sessão aparenta possuir um tempo de
expiração curto.
Uma investigação adicional seria necessária para medir o tempo de expiração
e determinar se o comportamento ocorre mesmo enquanto o usuário permanece ativo.

Oportunidade de melhoria.


### OBS04 — Dados do checkout não persistem ao retornar
Ao retornar de uma etapa posterior do checkout, os dados de nome, sobrenome
e ZIP Code precisam ser informados novamente.

Oportunidade de melhoria.

## Bugs encontrados

## Melhorias e dúvidas

## Bugs priorizados para regressão

## Riscos e pontos sem cobertura
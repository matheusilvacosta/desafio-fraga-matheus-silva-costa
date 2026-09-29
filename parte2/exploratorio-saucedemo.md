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
do catálogo, utilizando também o Inspetor/Ferramentas do Desenvolvedor do Firefox como apoio durante a exploração.

### 10 minutos — Preenchimento e tratamento de dados
Exploração dos campos utilizados durante o fluxo de compra, verificando como
a aplicação recebe e trata os valores informados pelo usuário.

### 10 minutos — Carrinho e fluxo de compra
Adição de itens ao carrinho, navegação pelo fluxo de compra e verificação das
informações apresentadas nas etapas de carrinho e checkout.

### 15 minutos — Reproduzir os bugs encontrados, passar anotações a limpo e entender quais bugs poderiam ser melhorias e vice-versa
Os comportamentos encontrados durante a exploração foram reproduzidos novamente
para confirmar sua consistência, impacto e passos necessários para reprodução.


## Observações da sessão, melhorias e dúvidas
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
### BUG-001 — Nome do produto exibe texto com aparência de função
Foi identificado conteúdo com aparência de código/função no nome de um produto
exibido no catálogo.

Report completo: `bugs/BUG-001.md`

### BUG-002 — Descrição do produto exibe texto com aparência de função
Foi identificado conteúdo com aparência de código/função na descrição de um
produto.

Report completo: `bugs/BUG-002.md`

### BUG-003 — Carrinho vazio não apresenta empty state adequado
Ao acessar o carrinho sem produtos, a página permanece praticamente vazia e
não apresenta uma mensagem clara indicando que não existem itens adicionados.

Report completo: `bugs/BUG-003.md`

### BUG-004 — Carrinho não permite alterar quantidade e não exibe imagens
Os produtos do carrinho são apresentados sem suas imagens e não existe um
controle disponível para alterar diretamente a quantidade de cada item.

Report completo: `bugs/BUG-004.md`

### BUG-005 — Checkout permite finalizar compra sem produtos
O sistema permite avançar pelo checkout e concluir uma compra mesmo quando
nenhum produto está presente no carrinho, gerando um pedido com total de $0.00.

Report completo: `bugs/BUG-005.md`

## Bugs priorizados para regressão
### 1. BUG-005 — Checkout permite finalizar compra sem produtos
Maior prioridade para regressão por afetar diretamente o fluxo principal de compra. O sistema permite gerar um pedido mesmo sem produtos associados, resultando em uma operação de valor $0.00.
Uma regressão nesse comportamento pode afetar regras importantes do processo de compra e geração de pedidos.

### 2. BUG-004 — Carrinho não permite alterar quantidade e não exibe imagens
Deve ser considerado em regressão por afetar uma etapa importante do fluxo de compra e a capacidade do usuário de revisar e gerenciar os produtos antes do checkout.

### 3. BUG-003 — Carrinho vazio não apresenta empty state adequado
Pode ser incluído na regressão do carrinho para garantir que o usuário receba um feedback adequado quando não possui produtos adicionados.

## Riscos e pontos sem cobertura
Devido ao tempo limitado dos testes exploratórios e devido ao teste ser sobre um site de demonstração, há alguns pontos importantes que não foram contemplados:

- comportamento em outros navegadores;
- comportamento em dispositivos móveis;
- validações completas dos campos do checkout (tratamento de erros validados);
- comportamento dos Dynamic Catalogs em todas as possíveis interações, devido as dúvidas se tal função está fazendo o esperado ou se está com erro;
- testes relacionados a desempenho;
- testes relacionados a acessibilidade;
- testes com outros tipos de contas.

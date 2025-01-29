# Sistema de Gerenciamento de Delivery

Este é um projeto desenvolvido para gerenciar pedidos, entregadores, restaurantes e clientes em um sistema de delivery. O sistema permite o registro de usuários, restaurantes, cardápios e pedidos, além de oferecer funcionalidades como avaliação de entregadores e atualização automática do total do pedido.

## Funcionalidades

- **Cadastro de Usuários**: Os usuários podem ser cadastrados como **clientes** ou **entregadores**.
- **Cadastro de Restaurantes**: Restaurantes podem ser cadastrados, com informações sobre categoria, endereço e dono.
- **Cadastro de Cardápio**: Cada restaurante tem um cardápio com itens que podem ser pedidos pelos clientes.
- **Gestão de Pedidos**: Clientes podem fazer pedidos nos restaurantes, que podem ser entregues por entregadores.
- **Avaliação**: Clientes podem avaliar entregadores e pedidos.
- **Triggers de Atualização**: O sistema possui triggers que atualizam automaticamente o total dos pedidos ao adicionar ou remover itens.

## Tecnologias Utilizadas

- **Banco de Dados**: MySQL
- **SQL**: Comandos de manipulação de dados (INSERT, SELECT, JOIN, etc.)
- **Triggers**: Automação de ações no banco de dados para atualizar valores automaticamente.
  
## Estrutura do Banco de Dados

O banco de dados contém as seguintes tabelas principais:

- **`usuario`**: Armazena informações sobre os usuários (clientes e entregadores).
- **`endereco`**: Guarda o endereço dos usuários.
- **`restaurante`**: Registra os restaurantes e suas informações.
- **`menu`**: Armazena os itens do cardápio de cada restaurante.
- **`pedido`**: Registra os pedidos realizados pelos clientes.
- **`item_pedido`**: Detalha os itens dentro de um pedido.
- **`avaliacao`**: Permite que os clientes avaliem os entregadores e os pedidos.
- **`status_pedido`**: Define os diferentes status de um pedido (pendente, em preparo, etc.).

## Testes Realizados

### 1. **Inserção de Dados Fictícios**
O primeiro teste consistiu em inserir dados fictícios nas tabelas `usuario`, `restaurante`, `menu`, `pedido`, `item_pedido`, entre outras. Isso foi feito para garantir que o banco de dados estivesse corretamente populado e as relações entre as tabelas estivessem funcionando.

### 2. **Consulta Simples - Seleção de Usuários**

Comando:
```sql
SELECT * FROM usuario;
```

![Captura de tela 2025-01-28 115314](https://github.com/user-attachments/assets/ac6860cb-b68c-4ffa-8390-444c4840465e)

**Objetivo**: Verificar se a tabela `usuario` estava corretamente populada com os dados dos usuários. Essa consulta retorna todos os registros da tabela `usuario`, permitindo visualizar os dados inseridos.

### 3. **Listar Restaurantes com suas Categorias**

Comando:
```sql
SELECT 
    restaurante.nome AS restaurante,
    categoria_restaurante.nome AS categoria,
    restaurante.cidade,
    restaurante.estado
FROM restaurante
INNER JOIN categoria_restaurante 
    ON restaurante.categoria_id = categoria_restaurante.id;
```

![Captura de tela 2025-01-28 115445](https://github.com/user-attachments/assets/818d8904-8f5e-467b-8a24-e5ce83bac232)


**Objetivo**: Verificar se a consulta retorna os restaurantes e suas respectivas categorias corretamente.

### 4. **Ver os Pedidos e os Clientes Responsáveis**

Comando:

```sql
SELECT 
    pedido.id AS pedido_id,
    usuario.nome AS cliente,
    status_pedido.nome AS status,
    pedido.total,
    pedido.data_criacao
FROM pedido
INNER JOIN usuario 
    ON pedido.cliente_id = usuario.id
INNER JOIN status_pedido 
    ON pedido.status_id = status_pedido.id;
```

![Captura de tela 2025-01-28 120332](https://github.com/user-attachments/assets/6bbdff3e-4f8a-479d-a390-f1603670452a)

**Objetivo**: Verificar se a consulta retorna os pedidos e os clientes responsáveis corretamente.

### 5. **Itens do Cardápio de Cada Restaurante**

Comando:

```sql
SELECT 
    restaurante.nome AS restaurante,
    menu.nome AS item,
    menu.preco,
    menu.disponivel
FROM menu
INNER JOIN restaurante 
    ON menu.restaurante_id = restaurante.id;
```

![Captura de tela 2025-01-28 120845](https://github.com/user-attachments/assets/eab5059e-0eb7-4400-a8fd-66a7f9edadd6)


**Objetivo**: Validar se a consulta retorna corretamente os itens do cardápio de cada restaurante.

### 6. **Detalhes de um Pedido Específico**

Comando SQL:

```sql
SELECT 
    pedido.id AS pedido_id,
    usuario.nome AS cliente,
    restaurante.nome AS restaurante,
    status_pedido.nome AS status,
    item_pedido.quantidade,
    item_pedido.preco_unitario,
    (item_pedido.quantidade * item_pedido.preco_unitario) AS subtotal
FROM pedido
INNER JOIN usuario 
    ON pedido.cliente_id = usuario.id
INNER JOIN restaurante 
    ON pedido.restaurante_id = restaurante.id
INNER JOIN status_pedido 
    ON pedido.status_id = status_pedido.id
INNER JOIN item_pedido 
    ON pedido.id = item_pedido.pedido_id;
```

![Captura de tela 2025-01-28 121006](https://github.com/user-attachments/assets/7a182d64-2f3b-4b5f-bd5c-817f972fd932)

**Objetivo**: Validar se a consulta retorna os detalhes de um pedido específico, incluindo itens e subtotal.

### 7. **Testando os Triggers - Inserção de Itens no Pedido**

**Comando SQL**:

```sql
-- Antes de inserir itens, analisar o total atual do pedido
SELECT 
    id AS pedido_id, 
    total 
FROM pedido
WHERE id = 1;

-- Adicionando um item ao pedido e verificando se o total será atualizado automaticamente:
INSERT INTO item_pedido (pedido_id, menu_id, quantidade, preco_unitario)
VALUES (1, 2, 3, 32.50); -- Adicionando 3 pizzas Marguerita ao pedido 1

-- Verificando se o total foi atualizado:
SELECT 
    id AS pedido_id, 
    total 
FROM pedido
WHERE id = 1;
```
*Antes de inserir itens:*

![Captura de tela 2025-01-28 121233](https://github.com/user-attachments/assets/051db32e-94fb-4c90-81a5-39a0b9715d51)

*Depois de inserir itens, verificando se foi atualizado:*

![Captura de tela 2025-01-28 121406](https://github.com/user-attachments/assets/9447c3ec-ca96-4c5c-aade-7377e59854f0)

**Objetivo**: Validar o funcionamento dos triggers de atualização do total do pedido após a inserção de um item.

### 8. **Pedidos com Entregadores Atribuídos**

Comando:

```sql
SELECT 
    pedido.id AS pedido_id,
    usuario.nome AS entregador,
    status_pedido.nome AS status,
    pedido.total,
    pedido.data_criacao
FROM pedido
INNER JOIN usuario 
    ON pedido.entregador_id = usuario.id
INNER JOIN status_pedido 
    ON pedido.status_id = status_pedido.id
WHERE pedido.entregador_id IS NOT NULL;
```

![Captura de tela 2025-01-28 122120](https://github.com/user-attachments/assets/12394db9-f6d3-455f-a506-eb5f519630cd)

**Objetivo**: Verificar se a consulta retorna corretamente os pedidos com entregadores atribuídos.

### 9. **Avaliações de Pedidos e Entregadores**

Comando:

```sql
SELECT 
    avaliacao.id AS avaliacao_id,
    pedido.id AS pedido_id,
    usuario.nome AS entregador,
    avaliacao.nota_pedido,
    avaliacao.nota_entregador,
    avaliacao.comentario,
    avaliacao.data_avaliacao
FROM avaliacao
INNER JOIN pedido 
    ON avaliacao.pedido_id = pedido.id
INNER JOIN usuario 
    ON avaliacao.entregador_id = usuario.id;
```

![Captura de tela 2025-01-28 122232](https://github.com/user-attachments/assets/f84df09e-8597-4dcb-b26e-4128854c5c03)

**Objetivo**: Validar se a consulta retorna corretamente as avaliações de pedidos e entregadores.

### Conclusão

Este projeto teve como objetivo desenvolver um sistema de gerenciamento de pedidos de delivery, com funcionalidades para gerenciar restaurantes, cardápios, pedidos, usuários (clientes e entregadores), status de pedidos, e avaliações de entregadores e pedidos.

Foram realizados diversos testes para validar a integridade dos dados e o funcionamento correto das funcionalidades, incluindo:

- Consultas para listar restaurantes, categorias, itens de cardápio e detalhes de pedidos.
- Validação de relacionamentos entre tabelas utilizando `JOIN`.
- Testes para garantir que os `triggers` de atualização do total de pedidos funcionam corretamente após a inserção e remoção de itens.
- Verificação de pedidos com entregadores atribuídos e de como as avaliações são registradas e recuperadas.

Com base nos testes realizados e nas consultas feitas, foi possível garantir que o sistema está operando de forma eficiente e correta, atendendo aos requisitos de um sistema de delivery completo, com a interação entre diferentes entidades e atualização dinâmica dos dados.

Este projeto foi um ótimo exercício para aplicar conceitos de banco de dados, SQL, triggers e integração entre diferentes tabelas e funcionalidades, e está pronto para ser expandido com mais recursos no futuro.

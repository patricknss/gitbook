# Modelagem simples de dados

Modelagem é a definição de entidades, atributos e relacionamentos antes de criar tabelas.

## Passos básicos

1. Identificar entidades (ex.: `Cliente`, `Pedido`, `Produto`).
2. Definir atributos (ex.: nome, email, data).
3. Definir chaves primárias e estrangeiras.
4. Normalizar para evitar redundância desnecessária.

## Exemplo mínimo

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE pedidos (
    id INT PRIMARY KEY,
    cliente_id INT NOT NULL,
    data_pedido DATE NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

## Resultado esperado

- Integridade referencial preservada.
- Estrutura simples para consultas e evolução.

## Exemplo de evolução

Para suportar múltiplos itens por pedido, crie uma tabela intermediária:

```sql
CREATE TABLE pedido_itens (
    pedido_id INT NOT NULL,
    produto_id INT NOT NULL,
    quantidade INT NOT NULL,
    PRIMARY KEY (pedido_id, produto_id)
);
```

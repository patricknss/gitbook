# Joins básicos

Joins combinam dados de duas ou mais tabelas relacionadas por uma chave.

## Tipos mais usados

- `INNER JOIN`: retorna apenas registros com correspondência nas duas tabelas.
- `LEFT JOIN`: retorna todos da tabela da esquerda e os correspondentes da direita.

## Exemplo

```sql
SELECT c.id, c.nome, p.numero
FROM clientes c
INNER JOIN pedidos p ON p.cliente_id = c.id;
```

## Filtros com join

```sql
SELECT c.nome, p.total
FROM clientes c
LEFT JOIN pedidos p ON p.cliente_id = c.id
WHERE p.total > 100;
```

## Dica prática

Use aliases curtos (`c`, `p`) e sempre explicite o `ON` para facilitar manutenção.

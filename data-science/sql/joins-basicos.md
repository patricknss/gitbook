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

### O que acontece aqui

- Apenas clientes com pedido aparecem.
- Cliente sem pedido fica fora do resultado.

## Filtros com join

```sql
SELECT c.nome, p.total
FROM clientes c
LEFT JOIN pedidos p ON p.cliente_id = c.id
WHERE p.total > 100;
```

### Observação importante

No `LEFT JOIN`, filtro no `WHERE` pode remover linhas nulas e aproximar o comportamento de `INNER JOIN`.

## Dica prática

Use aliases curtos (`c`, `p`) e sempre explicite o `ON` para facilitar manutenção.

## Exemplo de resultado esperado

Se `clientes` tiver `Ana` e `Bruno`, mas apenas `Ana` possuir pedido:

- `INNER JOIN` retorna apenas `Ana`.
- `LEFT JOIN` retorna `Ana` com pedido e `Bruno` com colunas de pedido nulas.

## Erros comuns

- Relacionar colunas erradas no `ON`.
- Usar `SELECT *` em join grande.
- Esquecer alias e dificultar manutenção.

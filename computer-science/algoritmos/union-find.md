# Union-Find (Disjoint Set Union)

Estrutura para gerenciar conjuntos disjuntos com eficiencia.

## Operacoes

- `find(x)`: encontra representante do conjunto.
- `union(a, b)`: une dois conjuntos.

## Otimizacoes

- path compression;
- union by rank/size.

Com essas otimizacoes, custo amortizado fica quase constante.

## Casos comuns

- detectar ciclo em grafo não dirigido;
- conectividade dinâmica;
- agrupamento por relacao.

## Veja tambem

- [Grafos (Fundamentos)](grafos-fundamentos.md)
- [Big O e Complexidade](big-o-e-complexidade.md)

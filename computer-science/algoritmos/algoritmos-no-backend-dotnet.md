# Algoritmos no Backend .NET

Como aplicar fundamentos de algoritmos em casos reais de API e servicos.

## Casos praticos

1. Deduplicacao de ids com `HashSet<T>`.
2. Lookup por chave com `Dictionary<TKey, TValue>`.
3. Top N com priority queue.
4. Pagina/processamento em lote para reduzir pico de memoria.

## LINQ com cuidado

- evitar enumeracao repetida da mesma query;
- evitar `Contains` em lista grande dentro de loop;
- projetar apenas campos necessarios.

## Escolha de estrutura (regra rapida)

- existencia: `HashSet`.
- chave/valor: `Dictionary`.
- ordenado por prioridade: `PriorityQueue`.
- fila de processamento: `Queue`.

## Checklist de performance

1. Qual complexidade do trecho critico?
2. Estrutura escolhida combina com padrao de acesso?
3. Ha alocacao desnecessaria?
4. Existe alternativa com custo menor?

## Veja tambem

- [Big O e Complexidade](big-o-e-complexidade.md)
- [Hash Table e Set](hash-table-e-set.md)
- [Persistencia: NHibernate x Dapper](../../back-end/backend/persistencia-nhibernate-dapper.md)

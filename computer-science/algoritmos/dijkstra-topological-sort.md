# Dijkstra e Topological Sort

Dois algoritmos clássicos de grafos para problemas diferentes.

## Dijkstra

- menor caminho a partir de uma origem;
- funciona com pesos não negativos;
- usa priority queue.

Complexidade comum: `O((V + E) log V)`.

## Topological sort

- ordena um DAG respeitando dependencias;
- usado em pipeline, build e workflow.

Complexidade: `O(V + E)`.

## Quando usar qual

- menor custo entre pontos: Dijkstra.
- ordem de execucao sem violar dependencia: topological sort.

## Veja tambem

- [Grafos (Fundamentos)](grafos-fundamentos.md)
- [Heap e Priority Queue](heap-e-priority-queue.md)

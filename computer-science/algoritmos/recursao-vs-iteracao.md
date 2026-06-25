# Recursao vs Iteracao

Recursao chama a propria funcao. Iteracao usa loops.

## Quando usar recursão

- Estruturas hierarquicas (arvore, DFS).
- Problemas naturalmente decomponiveis.

## Quando usar iteração

- Fluxo linear simples.
- Alto volume com controle de memória.

## Cuidado principal

Recursao profunda pode gerar stack overflow.

## Exemplo

```csharp
int FatorialRec(int n) => n <= 1 ? 1 : n * FatorialRec(n - 1);

int FatorialIt(int n)
{
    int r = 1;
    for (int i = 2; i <= n; i++) r *= i;
    return r;
}
```

## Veja tambem

- [BFS e DFS](bfs-e-dfs.md)
- [Arvores, BST e Trie](arvores-bst-e-trie.md)

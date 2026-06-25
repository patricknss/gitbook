# Big O e Complexidade

Big O descreve como o custo cresce com o tamanho da entrada.

## Notacoes mais usadas

| Notacao | Leitura | Exemplo |
| --- | --- | --- |
| `O(1)` | Constante | Acesso por indice |
| `O(log n)` | Logaritmica | Busca binaria |
| `O(n)` | Linear | Scan de lista |
| `O(n log n)` | Quase linear | Merge sort |
| `O(n^2)` | Quadratica | Dois loops |

## Tempo x espaco

- Complexidade de tempo: quantidade de operações.
- Complexidade de espaco: memória adicional usada.

## Regras práticas

1. Big O mostra tendencia, não tempo absoluto.
2. Constantes importam em produção, mas tendencia importa em escala.
3. Evite custos quadrados sem necessidade.

## Exemplo rápido

```csharp
// O(n^2)
foreach (var a in listaA)
    foreach (var b in listaB)
        if (a == b) { }

// O(n) + O(m)
HashSet<int> setB = listaB.ToHashSet();
foreach (var a in listaA)
    if (setB.Contains(a)) { }
```

## Veja tambem

- [Hash Table e Set](hash-table-e-set.md)
- [Busca e Ordenacao](busca-e-ordenacao.md)

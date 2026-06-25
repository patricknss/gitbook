# Prefix Sum e Difference Array

Tecnicas para responder range queries e atualizacoes em intervalo.

## Prefix sum

Preprocessa acumulado para consultar soma de intervalo em `O(1)`.

```text
soma(l..r) = prefix[r] - prefix[l-1]
```

## Difference array

Atualiza intervalos em `O(1)` por operação e reconstrui no final.

## Quando usar

- muitas consultas de intervalo;
- muitas atualizacoes por faixa;
- problemas de calendario, log e agregacao.

## Veja tambem

- [Arrays e Strings](arrays-e-strings.md)
- [Tecnicas: Two Pointers, Sliding Window e Hash Map](tecnicas-two-pointers-sliding-window-hashmap.md)

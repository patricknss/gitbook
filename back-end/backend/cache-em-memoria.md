# Gerenciamento de cache em memória

Cache em memória reduz chamadas repetidas a banco/API, guardando dados por um tempo no processo da aplicação.

## Quando usar

- Dados lidos com frequência e pouco alterados.
- Consultas caras (custo alto de banco/API).
- Cenários em que pequena defasagem de dados é aceitável.

## Exemplo com `IMemoryCache` (ASP.NET Core)

```csharp
public class ProdutosService
{
    private readonly IMemoryCache _cache;
    private readonly IProdutosRepository _produtosRepository;

    public ProdutosService(IMemoryCache cache, IProdutosRepository produtosRepository)
    {
        _cache = cache;
        _produtosRepository = produtosRepository;
    }

    public async Task<IReadOnlyCollection<Produto>> ListarAsync(CancellationToken cancellationToken)
    {
        const string cacheKey = "produtos:lista";

        if (_cache.TryGetValue(cacheKey, out IReadOnlyCollection<Produto> produtos))
            return produtos;

        produtos = await _produtosRepository.ListarAsync(cancellationToken);

        var options = new MemoryCacheEntryOptions()
            .SetAbsoluteExpiration(TimeSpan.FromMinutes(10))
            .SetSlidingExpiration(TimeSpan.FromMinutes(2))
            .SetPriority(CacheItemPriority.High);

        _cache.Set(cacheKey, produtos, options);
        return produtos;
    }
}
```

### Explicação do fluxo

1. Tenta ler do cache pela chave.
2. Se existir, devolve imediatamente (cache hit).
3. Se não existir, busca no repositório (cache miss).
4. Salva no cache com política de expiração e retorna.

## Invalidação de cache

Quando um dado for alterado, remova a chave relacionada:

```csharp
_cache.Remove("produtos:lista");
```

### Quando invalidar

- Após `INSERT`, `UPDATE` ou `DELETE` na entidade relacionada.
- Sempre que alteração afetar o resultado daquela chave de cache.

## Boas práticas

- Defina TTL explícito (`AbsoluteExpiration`).
- Evite cachear dados muito grandes sem limite.
- Use chaves padronizadas (`modulo:entidade:acao`).
- Monitore hit/miss para ajustar estratégia.

## Erros comuns

- Cachear sem TTL e servir dado desatualizado por muito tempo.
- Não invalidar após escrita.
- Usar mesma chave para filtros diferentes.
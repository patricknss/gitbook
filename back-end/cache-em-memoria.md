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

## Invalidação de cache

Quando um dado for alterado, remova a chave relacionada:

```csharp
_cache.Remove("produtos:lista");
```

## Boas práticas

- Defina TTL explícito (`AbsoluteExpiration`).
- Evite cachear dados muito grandes sem limite.
- Use chaves padronizadas (`modulo:entidade:acao`).
- Monitore hit/miss para ajustar estratégia.

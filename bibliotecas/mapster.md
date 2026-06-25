# Mapster

Mapster e uma biblioteca de mapeamento de objetos em .NET, usada para converter entidades em DTOs (e vice-versa) com foco em performance.

## Quando usar

- Transformar entidades de dominio em responses da API.
- Evitar mapeamento manual repetitivo entre camadas.
- Padronizar conversoes em projetos grandes.

## Instalacao

```bash
dotnet add package Mapster
```

## Exemplo basico

```csharp
using Mapster;

public class Produto
{
    public int Codigo { get; set; }
    public string Nome { get; set; }
}

public class ProdutoResponse
{
    public int Codigo { get; set; }
    public string Nome { get; set; }
}

Produto produto = new Produto { Codigo = 10, Nome = "Teclado" };
ProdutoResponse response = produto.Adapt<ProdutoResponse>();
```

## Veja tambem

- Uso no padrao de arquitetura: [DDD (Arquitetura Autoglass)](../back-end/ddd/arquitetura-autoglass-ddd.md)
- Base de API e DTOs: [ASP.NET Basico](../back-end/backend/aspnet-basico.md)
- Persistencia com ORMs: [NHibernate](nhibernate.md) e [Dapper](dapper.md)

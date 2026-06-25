# xUnit

xUnit e um framework de testes unitarios para .NET, muito usado para validar regras de negocio e prevenir regresses.

## Quando usar

- Testar entidades e servicos de dominio.
- Garantir comportamento esperado em cenarios criticos.
- Cobrir fluxos de sucesso, erro e borda.

## Instalacao

```bash
dotnet add package xunit
dotnet add package xunit.runner.visualstudio
```

## Exemplo basico

```csharp
using Xunit;

public class CalculadoraTests
{
    [Fact]
    public void Somar_DeveRetornarResultadoCorreto()
    {
        int resultado = 2 + 3;
        Assert.Equal(5, resultado);
    }
}
```

## Veja tambem

- Regras e padrao de dominio: [DDD (Arquitetura Autoglass)](../back-end/ddd/arquitetura-autoglass-ddd.md)
- APIs e services: [ASP.NET Basico](../back-end/backend/aspnet-basico.md)
- Bibliotecas de persistencia para mockar/validar em testes: [NHibernate](nhibernate.md) e [Dapper](dapper.md)

# NHibernate

NHibernate e um ORM para .NET que mapeia entidades para tabelas e abstrai operacoes de persistencia via sessao e consultas.

## Quando usar

- Projetos com dominio rico e mapeamento orientado a entidade.
- Cenarios que exigem controle de ciclo de vida da sessao.
- Repositorios com consultas via LINQ/Criteria/HQL.

## Instalacao

```bash
dotnet add package NHibernate
```

## Exemplo basico de consulta

```csharp
using NHibernate;

public class ClientesRepository
{
    private readonly ISession _session;

    public ClientesRepository(ISession session)
    {
        _session = session;
    }

    public Cliente Recuperar(int codigo)
    {
        return _session.Get<Cliente>(codigo);
    }
}
```

## Veja tambem

- Alternativa de micro-ORM: [Dapper](dapper.md)
- Arquitetura de dominio: [DDD (Arquitetura Autoglass)](../back-end/ddd/arquitetura-autoglass-ddd.md)
- Fundamentos SQL para consultas: [Banco de Dados](../data-science/sql/README.md)

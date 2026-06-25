# Dapper

Dapper e um micro-ORM para .NET que executa SQL diretamente com alta performance, fazendo mapeamento leve para objetos.

## Quando usar

- Consultas SQL com foco em desempenho.
- Projecoes e relatorios com queries especificas.
- Cenarios em que controle explicito de SQL e desejado.

## Instalacao

```bash
dotnet add package Dapper
```

## Exemplo basico de consulta

```csharp
using Dapper;
using System.Data;

public class ClientesRepository
{
    private readonly IDbConnection _connection;

    public ClientesRepository(IDbConnection connection)
    {
        _connection = connection;
    }

    public async Task<Cliente> RecuperarAsync(int codigo)
    {
        const string sql = "SELECT CODIGO, NOME FROM CLIENTE WHERE CODIGO = @Codigo";
        return await _connection.QueryFirstOrDefaultAsync<Cliente>(sql, new { Codigo = codigo });
    }
}
```

## Veja tambem

- ORM completo para comparacao: [NHibernate](nhibernate.md)
- Consultas e fundamentos relacionais: [Banco de Dados](../data-science/sql/README.md)
- Uso dentro do padrao de arquitetura: [DDD (Arquitetura Autoglass)](../back-end/ddd/arquitetura-autoglass-ddd.md)

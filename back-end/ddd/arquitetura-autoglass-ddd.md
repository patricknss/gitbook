# Domain-Driven Design - Arquitetura Autoglass

Este guia define o padrao DDD utilizado na Autoglass para .NET.

## Regras gerais

| Categoria | Regra |
| --- | --- |
| Nomenclatura | Classes de negocio em **portugues**: `Pedido`, `Cliente`, `NotaFiscal` |
| Nomenclatura | Padroes tecnicos em **ingles**: `UsuariosService`, `UsuariosRepository`, `UsuarioInstanciarCommand` |
| Parametros | Maximo de **7 parametros** por metodo/construtor; acima disso usar `Command` (servicos) ou `Filter` (repositorios) |
| Mapeamento | Usar **Mapster** (nunca AutoMapper) |
| Dependencias | Servico de dominio nao deve injetar repositorio de outra entidade quando existe servico disponivel |

## Estrutura de pastas recomendada

```text
src/
  Domain/
    Entidades/
    Services/
    Repositories/
  Application/
    Commands/
    Requests/
    Responses/
    Queries/
    Filters/
  Infrastructure/
    Repositories/
      NHibernate/
      Dapper/
  Api/
    Controllers/
```

## 1. Entidade

Regras obrigatorias:

1. Construtor vazio `protected`.
2. Propriedades `virtual` com `protected set`.
3. Construtor publico deve chamar `Set`.
4. Validacoes de regra sem banco de dados ficam na entidade.
5. Entidade nao recebe `Request` ou `Command`.

```csharp
public class Cliente
{
    public virtual int Codigo { get; protected set; }
    public virtual string Nome { get; protected set; }
    public virtual string Documento { get; protected set; }

    protected Cliente() { }

    public Cliente(string nome, string documento)
    {
        SetNome(nome);
        SetDocumento(documento);
    }

    public virtual void SetNome(string nome)
    {
        if (string.IsNullOrWhiteSpace(nome))
            throw new AtributoObrigatorioException(nameof(Nome));

        Nome = nome;
    }

    public virtual void SetDocumento(string documento)
    {
        if (string.IsNullOrWhiteSpace(documento))
            throw new AtributoObrigatorioException(nameof(Documento));

        Documento = documento;
    }
}
```

## 2. Servico de dominio

Nomenclatura: `{EntidadeNoPlural}Service` e `I{EntidadeNoPlural}Service`.

Metodos esperados:

1. `Instanciar`
2. `Inserir`
3. `Validar`
4. `Atualizar`

## 3. Command

Use quando servico ultrapassar 7 parametros.

```csharp
public class ClienteInstanciarCommand
{
    public string Nome { get; set; }
    public string Documento { get; set; }
}
```

## 4. Request

DTO de entrada da API. Nunca passar `Request` para servico de dominio.

```csharp
public class ClienteCadastroRequest
{
    public string Nome { get; set; }
    public string Documento { get; set; }
}
```

## 5. Response

DTO de saida da API.

```csharp
public class ClienteResponse
{
    public int Codigo { get; set; }
    public string Nome { get; set; }
    public string Documento { get; set; }
}
```

Mapeamento com Mapster:

```csharp
public static class ClienteMapper
{
    public static ClienteResponse ToResponse(this Cliente cliente)
    {
        return cliente.Adapt<ClienteResponse>();
    }
}
```

## 6. Repositorio

Nomenclatura: `{EntidadeNoPlural}Repository` e `I{EntidadeNoPlural}Repository`.

## 7. Filter

Use para filtros de repositorio com muitos parametros.

```csharp
public class ClienteListagemFilter
{
    public string Nome { get; set; }
    public string Documento { get; set; }
}
```

## 8. Query

Use para projecoes com dados de multiplas entidades.

```csharp
public class ClientesComPedidosQuery
{
    public int CodigoCliente { get; set; }
    public string NomeCliente { get; set; }
    public int QuantidadePedidos { get; set; }
}
```

## 9. Interfaces padrao

```csharp
public interface IClientesRepository
{
    void Inserir(Cliente cliente);
    void Atualizar(Cliente cliente);
    Cliente Recuperar(int codigo);
    IList<Cliente> Listar(ClienteListagemFilter filter);
}

public interface IClientesService
{
    Cliente Instanciar(string nome, string documento);
    void Inserir(Cliente cliente);
    Cliente Validar(int codigoCliente);
    void Atualizar(Cliente cliente, string nome, string documento);
}
```

## 10. Bibliotecas autorizadas

| Categoria | Biblioteca |
| --- | --- |
| Logging | Serilog |
| ORM | NHibernate, Dapper |
| Testes | XUnit, FluentAssertions, NSubstitute, NBuilder |
| JSON | Newtonsoft.Json |
| Mapeamento | Mapster |
| Excel | NPOI |
| PDF | Iron PDF |
| Retry / Circuit Break | Polly |
| Kafka | Confluent.Kafka |
| DI automatico | Scrutor |

## Referencias cruzadas

- Camada de API e services: [ASP.NET Basico](../backend/aspnet-basico.md)
- Persistencia relacional: [Banco de Dados](../../data-science/sql/README.md)
- Bibliotecas detalhadas: [Bibliotecas](../../bibliotecas/README.md)
- Mapeamento: [Mapster](../../bibliotecas/mapster.md)
- Persistencia: [NHibernate](../../bibliotecas/nhibernate.md) e [Dapper](../../bibliotecas/dapper.md)
- Testes unitarios: [xUnit](../../bibliotecas/xunit.md)

# Domain-Driven Design - Arquitetura Autoglass

Este guia organiza o padrao DDD da Autoglass por **camadas**, para ficar mais facil de aplicar no dia a dia.

## Visao arquitetural

```text
Api -> Application -> Domain -> Infrastructure
```

- **Api** recebe request HTTP e devolve response.
- **Application** orquestra entrada/saida (Request, Response, Command, Query).
- **Domain** concentra regras de negocio (Entidades, Services, Interfaces).
- **Infrastructure** executa persistencia (NHibernate/Dapper).

## Regras transversais (valem para todas as camadas)

| Tema | Regra |
| --- | --- |
| Linguagem ubiqua | Classes de negocio em portugues (`Cliente`, `Pedido`, `NotaFiscal`) |
| Padrao tecnico | Sufixos e padroes tecnicos em ingles (`ClientesService`, `ClientesRepository`) |
| Parametros | Maximo de 7 parametros por metodo/construtor |
| Acima de 7 parametros | Usar `Command` (service) ou `Filter` (repository) |
| Mapeamento | Usar **Mapster**, nunca AutoMapper |
| Dependencias | Nao injetar repositorio de outra entidade no service quando existir service dessa entidade |

---

## Camada Domain

### Entidades

Principios:

- Construtor vazio `protected`.
- Propriedades `virtual` com `protected set`.
- Construtor publico chama `Set...`.
- Validacoes sem acesso a banco ficam na entidade.
- Entidade nao recebe `Request` nem `Command`.

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

### Services de dominio

Padrao: `{EntidadeNoPlural}Service` com interface `I{EntidadeNoPlural}Service`.

Metodos base esperados:

- `Instanciar`
- `Inserir`
- `Validar`
- `Atualizar`

```csharp
public interface IClientesService
{
    Cliente Instanciar(string nome, string documento);
    void Inserir(Cliente cliente);
    Cliente Validar(int codigoCliente);
    void Atualizar(Cliente cliente, string nome, string documento);
}
```

### Interfaces de repositorio (contratos do dominio)

```csharp
public interface IClientesRepository
{
    void Inserir(Cliente cliente);
    void Atualizar(Cliente cliente);
    Cliente Recuperar(int codigo);
    IList<Cliente> Listar(ClienteListagemFilter filter);
}
```

---

## Camada Application

### Command

Use quando um metodo de service ultrapassar 7 parametros.

```csharp
public class ClienteInstanciarCommand
{
    public string Nome { get; set; }
    public string Documento { get; set; }
}
```

### Request

DTO da API para entrada. Nao deve atravessar para a camada Domain.

```csharp
public class ClienteCadastroRequest
{
    public string Nome { get; set; }
    public string Documento { get; set; }
}
```

### Response

DTO de saida da API.

```csharp
public class ClienteResponse
{
    public int Codigo { get; set; }
    public string Nome { get; set; }
    public string Documento { get; set; }
}
```

### Mapeamento com Mapster

```csharp
public static class ClienteMapper
{
    public static ClienteResponse ToResponse(this Cliente cliente)
    {
        return cliente.Adapt<ClienteResponse>();
    }
}
```

### Filter

Use para filtros de repositorio com muitos parametros.

```csharp
public class ClienteListagemFilter
{
    public string Nome { get; set; }
    public string Documento { get; set; }
}
```

### Query

Use para projecoes com campos de multiplas entidades.

```csharp
public class ClientesComPedidosQuery
{
    public int CodigoCliente { get; set; }
    public string NomeCliente { get; set; }
    public int QuantidadePedidos { get; set; }
}
```

---

## Camada Infrastructure

Implementa os contratos de repositorio do Domain.

- Pode usar **NHibernate** ou **Dapper**.
- Mantem regra de negocio fora da persistencia.
- Recebe `Filter` para cenarios de listagem mais complexos.

---

## Camada Api

- Recebe `Request`.
- Chama Application/Domain.
- Retorna `Response`.
- Nao concentra regra de negocio.

---

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

## Bibliotecas autorizadas

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

- Camada de API: [ASP.NET Basico](../backend/aspnet-basico.md)
- Persistencia relacional: [Banco de Dados](../../data-science/sql/README.md)
- Guia de bibliotecas: [Bibliotecas](../../bibliotecas/README.md)
- Mapeamento: [Mapster](../../bibliotecas/mapster.md)
- ORMs: [NHibernate](../../bibliotecas/nhibernate.md) e [Dapper](../../bibliotecas/dapper.md)
- Testes unitarios: [xUnit](../../bibliotecas/xunit.md)

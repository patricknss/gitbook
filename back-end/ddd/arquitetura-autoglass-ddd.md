# Domain-Driven Design - Arquitetura Autoglass

Este guia organiza o padrao DDD da Autoglass por **camadas**, para ficar mais facil de aplicar no dia a dia.

## Visao arquitetural

```text
Api -> Application -> Domain -> Infrastructure
         |              |
         v              v
     CrossCutting   Data Transfer
         |
         v
        IoC
         |
         v
        Jobs
         |
         v
       Tests
```

- **Api** recebe request HTTP e devolve response.
- **Application** orquestra entrada/saida (Request, Response, Command, Query).
- **Domain** concentra regras de negocio (Entidades, Services, Interfaces).
- **Infrastructure** executa persistencia (NHibernate/Dapper).
- **Data Transfer** define contratos de transporte entre camadas (Request/Response/Command/Query).
- **CrossCutting** centraliza preocupacoes transversais (log, resiliencia, validacao, observabilidade, seguranca).
- **IoC** registra dependencias e composicao de modulos.
- **Jobs** executam fluxos assincronos e tarefas agendadas.
- **Tests** validam regras de negocio e comportamento das camadas.

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

### Data Transfer (DTOs e contratos)

Nesta arquitetura, os objetos de transferencia ficam na borda da aplicacao:

- `Request`: entrada da API.
- `Response`: saida da API.
- `Command`: entrada para casos de uso com mais dados.
- `Query`: projecoes de leitura.
- `Filter`: criterio de busca para repositorios.

Regras:

- Nao carregar regra de negocio nesses objetos.
- Nao usar `Request` diretamente em services de dominio.
- Usar Mapster para mapear Domain -> Response.

---

## Camada Infrastructure

Implementa os contratos de repositorio do Domain.

- Pode usar **NHibernate** ou **Dapper**.
- Mantem regra de negocio fora da persistencia.
- Recebe `Filter` para cenarios de listagem mais complexos.

---

## Camada CrossCutting

Responsavel por componentes compartilhados entre modulos:

- Logging (Serilog)
- Resiliencia (Polly)
- Notificacao/mensageria (Kafka, quando aplicavel)
- Utilitarios comuns (excecoes base, helpers, extensoes)
- Politicas de validacao e tratamento de erro padronizado

Objetivo: evitar duplicacao e manter padrao tecnico unico em toda a solucao.

---

## IoC (Inversao de Controle)

A composicao da aplicacao deve registrar interfaces e implementacoes por camada:

- `IClientesService` -> `ClientesService`
- `IClientesRepository` -> `ClientesRepository`
- Mappers, validadores e componentes de CrossCutting

Com isso, a API depende de contratos, e nao de implementacoes concretas.

---

## Jobs (processamento assincrono e agendado)

Use Jobs para fluxos fora da requisicao HTTP:

- Reprocessamentos
- Integracoes externas
- Rotinas agendadas
- Consumo de filas/eventos

Boas praticas:

- Job orquestra, mas regra de negocio continua no Domain/Application.
- Dependencias resolvidas via IoC.
- Erros e metricas reportados via CrossCutting.

---

## Tests (estrategia de testes)

Prioridades de cobertura:

- **Domain**: testes unitarios de entidade e service de dominio.
- **Application**: testes de casos de uso (commands/queries).
- **Infrastructure**: testes de repositorio quando necessario.
- **Api**: testes de contrato/endpoint para fluxos principais.

Ferramental recomendado no padrao Autoglass:

- xUnit
- FluentAssertions
- NSubstitute
- NBuilder

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
  CrossCutting/
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
    DataTransfer/
  Infrastructure/
    Repositories/
      NHibernate/
      Dapper/
  Jobs/
  Api/
    Controllers/
  Ioc/
  Tests/
    Unit/
    Integration/
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

# Domain-Driven Design - Arquitetura Autoglass

Este guia organiza o padrao DDD da Autoglass na **ordem em que normalmente desenvolvemos um sistema**.

## Ordem de desenvolvimento das camadas

1. Dominio
2. DataTransfer
3. Aplicacao
4. Infra
5. CrossCutting
6. IoC
7. API
8. Jobs
9. Tests

## Visao arquitetural (execucao)

```text
API -> Application -> Domain
           |
           v
     Infrastructure
```

> Em tempo de desenvolvimento, seguimos a ordem acima (Dominio ate Tests).  
> Em tempo de execucao, a API chama a Aplicacao, que aplica regras de Dominio e usa Infra.

## Regras transversais (valem para todas as camadas)

| Tema | Regra |
| --- | --- |
| Linguagem ubiqua | Classes de negocio em portugues (`Cliente`, `Pedido`, `NotaFiscal`) |
| Padrao tecnico | Sufixos em ingles (`ClientesService`, `ClientesRepository`) |
| Parametros | Maximo de 7 parametros por metodo/construtor |
| Acima de 7 parametros | Usar `Command` (service) ou `Filter` (repository) |
| Mapeamento | Usar **Mapster**, nunca AutoMapper |
| Dependencias | Nao injetar repositorio de outra entidade no service quando existir service dessa entidade |

---

## A camada de Dominio

Como esta descrito acima, ela e o coracao do sistema, onde vive a inteligencia do negocio e todas as suas regras.

### Entidades

- Construtor vazio `protected`
- Propriedades `virtual` com `protected set`
- Construtor publico chama `Set...`
- Validacoes sem acesso a banco ficam na entidade
- Entidade nao recebe `Request` nem `Command`

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

## A camada de DataTransfer

Uma camada usada para os nossos DTOs (Data Transfer Objects), servindo para transportar informacoes necessarias entre as camadas.

Elementos comuns:

- `Request`: entrada da API
- `Response`: saida da API
- `Command`: entrada para casos de uso com muitos dados
- `Query`: projecoes de leitura
- `Filter`: criterio de busca para repositorios

Regras:

- Nao carregar regra de negocio nesses objetos
- Nao usar `Request` diretamente em services de dominio
- Usar composicao quando necessario

```csharp
public class ClienteCadastroRequest
{
    public string Nome { get; set; }
    public string Documento { get; set; }
}

public class ClienteResponse
{
    public int Codigo { get; set; }
    public string Nome { get; set; }
    public string Documento { get; set; }
}

public class ClienteInstanciarCommand
{
    public string Nome { get; set; }
    public string Documento { get; set; }
}

public class ClienteListagemFilter
{
    public string Nome { get; set; }
    public string Documento { get; set; }
}
```

---

## A camada Aplicacao

Esta camada e 1 antes da API. Ela atua como orquestradora entre as outras camadas para receber informacoes vindas externamente, tratar internamente e devolver uma resposta.

Responsabilidades:

- Orquestrar casos de uso
- Chamar services de dominio
- Converter fluxo de entrada/saida (sem regra de negocio pesada)
- Nao acessar banco diretamente

---

## A camada de Infra

Nesta camada, criamos a implementacao do repositorio e tambem o mapeamento.

Responsabilidades:

- Implementar contratos de repositorio do Domain
- Persistencia com NHibernate/Dapper
- Mapeamentos tecnicos (Mapster)

```csharp
public static class ClienteMapper
{
    public static ClienteResponse ToResponse(this Cliente cliente)
    {
        return cliente.Adapt<ClienteResponse>();
    }
}
```

---

## A camada de CrossCutting

Camada de preocupacoes transversais, com componentes compartilhados como log, resiliencia, seguranca, observabilidade e tratamento padronizado de erros.

Exemplos:

- Logging (Serilog)
- Resiliencia (Polly)
- Notificacao/mensageria (Kafka, quando aplicavel)
- Excecoes base, helpers, extensoes

---

## A camada de IoC (Inversao de Controle)

Onde configuramos injecao de dependencia e composicao de modulos, ligando interfaces as implementacoes concretas.

Exemplos de registro:

- `IClientesService` -> `ClientesService`
- `IClientesRepository` -> `ClientesRepository`
- Mappers, validadores e componentes de CrossCutting

---

## A camada de API

Esta e a ultima camada, a parte externa onde interagimos com a aplicacao.

Responsabilidades:

- Receber `Request`
- Chamar Application/Domain
- Retornar `Response`
- Nao concentrar regra de negocio

---

## A camada de Jobs

Responsavel por processamentos assincronos e tarefas agendadas fora da requisicao HTTP.

Uso comum:

- Reprocessamentos
- Integracoes externas
- Rotinas agendadas
- Consumo de filas/eventos

---

## A camada de Tests

Camada de validacao do comportamento da aplicacao, com foco principal em testes de Dominio e cobertura dos fluxos criticos.

Prioridades:

- **Domain**: testes unitarios de entidade e service de dominio
- **Application**: testes de casos de uso
- **Infrastructure**: testes de repositorio quando necessario
- **API**: testes de contrato/endpoint

Ferramental recomendado:

- xUnit
- FluentAssertions
- NSubstitute
- NBuilder

---

## Estrutura de pastas recomendada

```text
src/
  Domain/
    Entidades/
    Services/
    Repositories/
  DataTransfer/
    Requests/
    Responses/
    Commands/
    Queries/
    Filters/
  Application/
  Infrastructure/
    Repositories/
      NHibernate/
      Dapper/
    Mappers/
  CrossCutting/
  Ioc/
  Api/
    Controllers/
  Jobs/
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

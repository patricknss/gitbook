# Domain-Driven Design - Arquitetura Autoglass

Este guia organiza a arquitetura DDD no padrao Autoglass. O exemplo principal e a entidade `Aluno`, porque e o exemplo usado no material.

DDD nao e so estrutura de pastas. A ideia central e modelar o software a partir do conhecimento do negocio, usando uma linguagem que o time tecnico e o time de negocio consigam reconhecer.

## Objetivo do DDD

- Modelar a realidade do negocio.
- Criar software que fala a linguagem da empresa.
- Traduzir regras complexas com clareza.
- Reduzir acoplamento acidental entre camadas.
- Manter o dominio como o core do sistema.

## Ordem recomendada de desenvolvimento

1. `Dominio`: entidade, enumeradores, contratos de repositorio, comandos e servicos de dominio.
2. `Infra`: mapeamentos NHibernate, implementacoes de repositorio e consultas tecnicas.
3. `DataTransfer`: requests e responses usados nas bordas do sistema.
4. `Aplicacao`: app services, transacoes, logs e mapeamentos entre DTOs e dominio.
5. `IoC`: registro de dependencias.
6. `API`: controllers e endpoints HTTP.
7. `Jobs`: rotinas assincronas/agendadas, quando existirem.
8. `Tests`: cobertura das regras e dos fluxos principais.

## Fluxo de execucao

```text
API -> Aplicacao -> Dominio -> Repositorio (contrato)
                         |
                         v
                       Infra -> Banco
```

Em operacoes de escrita, a transacao normalmente fica na camada de Aplicacao. O Dominio decide o comportamento de negocio, mas nao abre transacao, nao sabe de HTTP e nao conhece detalhes do banco.

## Regras transversais

| Tema | Regra |
| --- | --- |
| Linguagem ubiqua | Classes de negocio em portugues: `Aluno`, `Usuario`, `Certificado`. |
| Pastas por contexto | Pastas do agregado/contexto normalmente no plural: `Alunos`, `Usuarios`, `Certificados`. |
| Nome da entidade | Prefira entidade no singular: `Aluno`. O PDF alterna `Aluno` e `Alunos`; escolha uma convencao e use ate o fim. |
| Padrao tecnico | Interfaces com `I`; classes tecnicas com sufixo claro: `AlunosServico`, `AlunosRepositorio`, `AlunosMap`. |
| Parametros | Evite metodos/construtores com mais de 7 parametros. |
| Muitos parametros | Use `Comando` em servicos de dominio e `Filtro` em consultas/repositorios. |
| Mapeamento | Neste GitBook usamos **Mapster**. Se o projeto legado usar AutoMapper, traduza os `Profiles` do PDF para configuracoes Mapster. |
| Dependencias | Se existir service de uma entidade, injete o service, nao o repositorio dessa entidade. |
| Controller | Controller deve ser fino: recebe request, delega para Aplicacao e retorna HTTP. |
| Entidade na API | Nao exponha entidade de dominio diretamente como response. |
| Erro em transacao | Em `catch`, faca `Rollback`; `Commit` so no caminho feliz. |

## Estrutura de pastas por topico

Use esta estrutura como mapa mental para entender onde cada assunto da arquitetura entra. Ela e uma visao generica: na pratica, o projeto pode usar nomes como `Autoglass.Projeto.Dominio`, `Autoglass.Projeto.Aplicacao` e assim por diante.

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

### `Domain/`

`Domain` guarda o que representa o negocio. Aqui ficam as entidades, as regras e os contratos que o dominio precisa para persistir ou consultar dados.

`Entidades/`: classes que representam conceitos do negocio, como `Aluno`, `Usuario`, `Certificado` e `Professor`. Elas concentram propriedades, construtores e metodos `Set...` com regras do proprio objeto.

`Services/`: servicos de dominio, como `AlunosServico`. Eles coordenam regras que envolvem mais de uma operacao ou dependencia de negocio, por exemplo validar usuario, instanciar aluno, inserir, editar ou inativar.

`Repositories/`: interfaces de repositorio, como `IAlunosRepositorio`. O dominio declara o contrato, mas nao implementa acesso ao banco.

### `Application/`

`Application` organiza os casos de uso e os objetos de entrada e saida que circulam entre API e dominio.

`Commands/`: objetos usados para executar uma acao, como `AlunoInserirComando` ou `ProfessorEditarComando`. Quando o projeto seguir o modelo do PDF, esses comandos podem aparecer dentro do proprio `Domain/<Contexto>/Comandos`; a ideia e a mesma.

`Requests/`: dados que chegam pela API, como `AlunoInserirRequest`.

`Responses/`: dados que voltam para o cliente, como `AlunoResponse`.

`Queries/`: projecoes de leitura, usadas quando a resposta nao precisa carregar uma entidade inteira.

`Filters/`: criterios de consulta, paginacao e ordenacao, como `AlunoListarFiltro`.

O App Service tambem fica nessa camada, normalmente em uma pasta como `Services/` ou `Servicos/`. Ele abre transacao, chama o dominio, registra logs e converte request/comando/response.

### `Infrastructure/`

`Infrastructure` guarda os detalhes tecnicos de persistencia e integracoes. Ela implementa os contratos que o dominio declarou.

`Repositories/NHibernate/`: repositorios concretos que usam NHibernate, como `AlunosRepositorio : RepositorioNHibernate<Aluno>, IAlunosRepositorio`.

`Repositories/Dapper/`: consultas SQL diretas para listagens, relatorios ou cenarios onde uma projecao com Dapper e mais simples e performatica.

Tambem pode existir uma pasta de `Mapeamentos/` quando o projeto usa Fluent NHibernate, por exemplo `AlunosMap`.

### `Api/`

`Api` e a borda HTTP.

`Controllers/`: controllers como `AlunosController`. Eles recebem request, chamam o app service e retornam status HTTP. Controller nao chama repositorio, nao instancia entidade e nao concentra regra de negocio.

---

## 01 - Camada de Dominio

A camada de Dominio e o coracao do sistema. Nela ficam as entidades, as regras de negocio, os enumeradores, os comandos de caso de uso do dominio, as interfaces de repositorio e os servicos de dominio.

Estrutura base:

```text
Autoglass.Projeto.Dominio/
  Alunos/
    Entidades/
      Aluno.cs
    Enumeradores/
    Repositorios/
      IAlunosRepositorio.cs
    Comandos/
      AlunoInserirComando.cs
      AlunoEditarComando.cs
    Servicos/
      Interfaces/
        IAlunosServico.cs
      AlunosServico.cs
  Utils/
    Enumeradores/
      AtivoInativoEnum.cs
```

### Entidades

A entidade representa o conceito do negocio e protege suas invariantes. No exemplo do PDF, `Aluno` possui `Usuario` e `Status`.

Regras para entidades:

- Propriedades devem ser `virtual` com `protected set`.
- O construtor publico nao define o `Id`, porque ele sera tratado pelo banco.
- A entidade precisa de construtor vazio `protected` para o NHibernate.
- O construtor publico deve chamar metodos `Set...`, em vez de atribuir direto.
- Metodos `Set...` concentram as validacoes e regras de negocio do atributo.
- A entidade nao recebe `Request`, nao recebe `Response` e nao conhece API.

```csharp
using Autoglass.Projeto.Dominio.Usuarios.Entidades;
using Autoglass.Projeto.Dominio.Utils.Enumeradores;
using Autoglass.Libs.Dominio.Excecoes;

namespace Autoglass.Projeto.Dominio.Alunos.Entidades
{
    public class Aluno
    {
        public virtual int Id { get; protected set; }
        public virtual Usuario Usuario { get; protected set; }
        public virtual AtivoInativoEnum Status { get; protected set; }

        protected Aluno()
        {
        }

        public Aluno(Usuario usuario)
        {
            SetUsuario(usuario);
            SetStatus(AtivoInativoEnum.Ativo);
        }

        public virtual void SetUsuario(Usuario usuario)
        {
            if (usuario is null)
                throw new RegraDeNegocioExcecao("Usuario nao informado");

            Usuario = usuario;
        }

        public virtual void SetStatus(AtivoInativoEnum status)
        {
            Status = status;
        }
    }
}
```

### Enumeradores

Existem dois tipos comuns de enumeradores.

Enumerador especifico de um contexto:

```text
Autoglass.Projeto.Dominio/
  Certificados/
    Entidades/
      Certificado.cs
    Enumeradores/
      StatusCertificadoEnum.cs
```

```csharp
using System.ComponentModel;

namespace Autoglass.Projeto.Dominio.Certificados.Enumeradores
{
    public enum StatusCertificadoEnum
    {
        [Description("Nao iniciado")]
        NaoIniciado = 0,

        [Description("Em andamento")]
        EmAndamento = 1,

        [Description("Concluido")]
        Concluido = 2,

        [Description("Cancelado")]
        Cancelado = 3
    }
}
```

Enumerador compartilhado pelo sistema:

```text
Autoglass.Projeto.Dominio/
  Utils/
    Enumeradores/
      AtivoInativoEnum.cs
```

```csharp
namespace Autoglass.Projeto.Dominio.Utils.Enumeradores
{
    public enum AtivoInativoEnum
    {
        Inativo = 0,
        Ativo = 1
    }
}
```

### Contratos de repositorio

No Dominio ficam apenas os contratos. A implementacao concreta fica na Infra.

```csharp
using Autoglass.Projeto.Dominio.Alunos.Entidades;
using Autoglass.Libs.Dominio.Repositorios;

namespace Autoglass.Projeto.Dominio.Alunos.Repositorios
{
    public interface IAlunosRepositorio : IRepositorioNHibernate<Aluno>
    {
    }
}
```

A interface herdada normalmente ja oferece os metodos basicos do NHibernate:

```csharp
public interface IRepositorioNHibernate<T> where T : class
{
    void Inserir(T entidade);
    void Inserir(IEnumerable<T> entidades);
    void Editar(T entidade);
    void Excluir(T entidade);
    void Excluir(IEnumerable<T> entidades);
    T Recuperar(int id);
    T Recuperar(Expression<Func<T, bool>> expression);
    IQueryable<T> Query();
    PaginacaoConsulta<T> Listar(IQueryable<T> query, int qt, int pg, string cpOrd, TipoOrdenacaoEnum tpOrd);
    void Refresh(T entidade);
}
```

Use o contrato especifico (`IAlunosRepositorio`) para permitir injecao de dependencia sem acoplar o Dominio a uma classe concreta.

### Comandos

Um comando agrupa os dados necessarios para executar uma operacao no service de dominio. Ele evita metodos com muitos parametros e deixa claro o caso de uso.

Exemplo ruim:

```csharp
public Professor Inserir(string areaAtuacao, int idUsuario)
{
    // ...
}
```

Exemplo melhor:

```csharp
namespace Autoglass.Projeto.Dominio.Professores.Comandos
{
    public class ProfessorInserirComando
    {
        public string AreaAtuacao { get; set; }
        public int IdUsuario { get; set; }
    }
}
```

```csharp
public Professor Inserir(ProfessorInserirComando comando)
{
    // ...
}
```

Regras para comandos:

- O comando fica no Dominio quando ele e entrada de um service de dominio.
- Os nomes e tipos devem ser compativeis com a operacao que sera executada.
- Se a entidade possui referencia para outra entidade (`Usuario`), o comando recebe `int IdUsuario`.
- Nao coloque regra de negocio dentro do comando.
- Se `Request` e `Comando` representam a mesma entrada, mantenha propriedades com nomes e tipos identicos para facilitar o mapeamento.

### Servicos de dominio

O service de dominio manipula entidades e aplica regras de negocio que nao pertencem a uma unica propriedade da entidade.

Estrutura:

```text
Autoglass.Projeto.Dominio/
  Alunos/
    Servicos/
      Interfaces/
        IAlunosServico.cs
      AlunosServico.cs
```

Interface:

```csharp
using Autoglass.Projeto.Dominio.Alunos.Comandos;
using Autoglass.Projeto.Dominio.Alunos.Entidades;

namespace Autoglass.Projeto.Dominio.Alunos.Servicos.Interfaces
{
    public interface IAlunosServico
    {
        Aluno Instanciar(AlunoInserirComando comando);
        Aluno Inserir(AlunoInserirComando comando);
        Aluno Validar(int id);
        void Inativar(int id);
    }
}
```

Implementacao:

```csharp
using Autoglass.Projeto.Dominio.Alunos.Comandos;
using Autoglass.Projeto.Dominio.Alunos.Entidades;
using Autoglass.Projeto.Dominio.Alunos.Repositorios;
using Autoglass.Projeto.Dominio.Alunos.Servicos.Interfaces;
using Autoglass.Projeto.Dominio.Usuarios.Entidades;
using Autoglass.Projeto.Dominio.Usuarios.Servicos.Interfaces;
using Autoglass.Projeto.Dominio.Utils.Enumeradores;
using Autoglass.Libs.Dominio.Excecoes;

namespace Autoglass.Projeto.Dominio.Alunos.Servicos
{
    public class AlunosServico : IAlunosServico
    {
        private readonly IAlunosRepositorio alunosRepositorio;
        private readonly IUsuariosServico usuariosServico;

        public AlunosServico(
            IAlunosRepositorio alunosRepositorio,
            IUsuariosServico usuariosServico)
        {
            this.alunosRepositorio = alunosRepositorio;
            this.usuariosServico = usuariosServico;
        }

        public Aluno Inserir(AlunoInserirComando comando)
        {
            Aluno aluno = Instanciar(comando);
            alunosRepositorio.Inserir(aluno);
            return aluno;
        }

        public Aluno Instanciar(AlunoInserirComando comando)
        {
            Usuario usuario = usuariosServico.Validar(comando.IdUsuario);
            return new Aluno(usuario);
        }

        public Aluno Validar(int id)
        {
            Aluno aluno = alunosRepositorio.Recuperar(id);

            if (aluno is null)
                throw new RegraDeNegocioExcecao("Aluno nao localizado");

            return aluno;
        }

        public void Inativar(int id)
        {
            Aluno aluno = Validar(id);
            aluno.SetStatus(AtivoInativoEnum.Inativo);
            alunosRepositorio.Editar(aluno);
        }
    }
}
```

Fluxo comum dos metodos:

```text
Validar -> Instanciar -> Inserir
Validar -> Editar
Validar -> Inativar
```

Responsabilidades tipicas:

- `Validar`: recuperar pelo id e garantir que a entidade existe.
- `Instanciar`: montar uma entidade valida a partir de um comando.
- `Inserir`: instanciar e persistir.
- `Editar`: validar, alterar via `Set...` e persistir.
- `Inativar`: validar, alterar status e persistir.

O service de dominio pode chamar outro service de dominio (`IUsuariosServico`) para validar uma dependencia de negocio. Evite buscar `Usuario` diretamente pelo `IUsuariosRepositorio` se ja existir service de usuario.

---

## 02 - Camada de Infra

A Infra implementa os contratos tecnicos que o Dominio definiu. Ela conhece NHibernate, Dapper, sessoes, mapeamentos e detalhes do banco.

Estrutura:

```text
Autoglass.Projeto.Infra/
  Alunos/
    Mapeamentos/
      AlunosMap.cs
    Repositorios/
      AlunosRepositorio.cs
```

### Mapeamentos NHibernate

O mapeamento informa ao NHibernate como a entidade se relaciona com schema, tabela, coluna, sequence, enum e referencias.

```csharp
using Autoglass.Projeto.Dominio.Alunos.Entidades;
using Autoglass.Projeto.Dominio.Utils.Enumeradores;
using FluentNHibernate.Mapping;

namespace Autoglass.Projeto.Infra.Alunos.Mapeamentos
{
    public class AlunosMap : ClassMap<Aluno>
    {
        public AlunosMap()
        {
            Schema("PROJETO");
            Table("ALUNO");

            Id(x => x.Id)
                .Column("ID")
                .GeneratedBy.Sequence("SEQ_ALUNO");

            Map(x => x.Status)
                .Column("STATUS")
                .CustomType<AtivoInativoEnum>();

            References(x => x.Usuario)
                .Column("IDUSUARIO");
        }
    }
}
```

Checklist do mapeamento:

- Herdar de `ClassMap<Entidade>`.
- Informar `Schema`.
- Informar `Table`.
- Mapear `Id` e sequence, quando o banco usar sequence.
- Mapear propriedades simples com `Map`.
- Mapear enum com `CustomType<Enum>()`, quando necessario.
- Mapear referencias com `References`.

### Repositorios concretos

O repositorio concreto fica na Infra. Ele implementa a interface do Dominio e herda a base pronta do NHibernate.

```csharp
using Autoglass.Projeto.Dominio.Alunos.Entidades;
using Autoglass.Projeto.Dominio.Alunos.Repositorios;
using Autoglass.Libs.NHibernate.Repositorios;
using NHibernate;

namespace Autoglass.Projeto.Infra.Alunos.Repositorios
{
    public class AlunosRepositorio : RepositorioNHibernate<Aluno>, IAlunosRepositorio
    {
        public AlunosRepositorio(ISession session) : base(session)
        {
        }
    }
}
```

Essa classe parece pequena porque a base `RepositorioNHibernate<T>` ja implementa operacoes como `Inserir`, `Editar`, `Excluir`, `Recuperar`, `Query`, `Listar` e `Refresh`.

### Consultas com Dapper

Use Dapper na Infra quando a consulta for mais natural em SQL direto, principalmente para relatorios, listagens otimizadas ou projecoes de leitura.

Regras:

- Dapper nao substitui regra de negocio do Dominio.
- SQL fica na Infra.
- Retorne DTO de leitura, `Query` ou objeto de projecao quando nao fizer sentido hidratar a entidade inteira.
- Parametrize sempre as queries.

```csharp
public class AlunosConsultaRepositorio
{
    private readonly IDbConnection connection;

    public AlunosConsultaRepositorio(IDbConnection connection)
    {
        this.connection = connection;
    }

    public Task<IEnumerable<AlunoResumoQuery>> ListarResumos()
    {
        const string sql = @"
            SELECT
                A.ID AS Id,
                A.IDUSUARIO AS IdUsuario,
                A.STATUS AS Status
            FROM PROJETO.ALUNO A";

        return connection.QueryAsync<AlunoResumoQuery>(sql);
    }
}
```

---

## 03 - Camada de DataTransfer

DataTransfer contem os objetos usados para transportar dados entre a borda externa e a Aplicacao. No PDF, ela aparece principalmente com `Requests` e `Responses`.

Estrutura:

```text
Autoglass.Projeto.DataTransfer/
  Alunos/
    Requests/
      AlunoInserirRequest.cs
    Responses/
      AlunoResponse.cs
    Queries/
      AlunoResumoQuery.cs
    Filtros/
      AlunoListarFiltro.cs
```

### Requests

`Request` representa entrada externa, normalmente vinda da API por `GET`, `POST`, `PUT`, `PATCH` ou `DELETE`.

```csharp
namespace Autoglass.Projeto.DataTransfer.Alunos.Requests
{
    public class AlunoInserirRequest
    {
        public int IdUsuario { get; set; }
    }
}
```

Exemplo com mais campos:

```csharp
using Autoglass.Projeto.Dominio.Certificados.Enumeradores;

namespace Autoglass.Projeto.DataTransfer.Certificados.Requests
{
    public class CertificadoEditarRequest
    {
        public int Id { get; set; }
        public string NomeDoCurso { get; set; }
        public decimal CargaHoraria { get; set; }
        public DateTime DataEmissao { get; set; }
        public string Instituicao { get; set; }
        public StatusCertificadoEnum Status { get; set; }
    }
}
```

Regras:

- Request nao tem regra de negocio.
- Request nao acessa repositorio.
- Request nao entra direto na entidade.
- Quando existir comando correspondente, mantenha nomes e tipos iguais para facilitar o mapeamento.
- Validacoes de formato podem acontecer na API ou em validadores; validacao de negocio fica no Dominio.

### Responses

`Response` representa a saida enviada ao cliente da API.

```csharp
namespace Autoglass.Projeto.DataTransfer.Alunos.Responses
{
    public class AlunoResponse
    {
        public int Id { get; set; }
        public int IdUsuario { get; set; }
        public int Status { get; set; }
    }
}
```

Regras:

- Response nao deve expor entidade de dominio diretamente.
- Pode achatar dados de entidades relacionadas, como `IdUsuario`.
- Pode esconder propriedades internas que nao devem sair na API.
- Pode ser diferente da entidade quando a resposta precisar de outro formato.

### Queries e filtros

Use `Query` para retorno de leitura que nao representa uma entidade completa.

```csharp
namespace Autoglass.Projeto.DataTransfer.Alunos.Queries
{
    public class AlunoResumoQuery
    {
        public int Id { get; set; }
        public int IdUsuario { get; set; }
        public string NomeUsuario { get; set; }
        public int Status { get; set; }
    }
}
```

Use `Filtro` quando uma consulta receber muitos criterios.

```csharp
namespace Autoglass.Projeto.DataTransfer.Alunos.Filtros
{
    public class AlunoListarFiltro
    {
        public int? IdUsuario { get; set; }
        public int? Status { get; set; }
        public int Pagina { get; set; }
        public int Quantidade { get; set; }
    }
}
```

---

## 04 - Camada de Aplicacao

A Aplicacao orquestra o fluxo entre API, DataTransfer, Dominio, Infra e recursos transversais. Ela nao e o lugar principal de regra de negocio; ela coordena caso de uso.

Estrutura:

```text
Autoglass.Projeto.Aplicacao/
  Alunos/
    Mapeamentos/
      AlunosMapsterConfig.cs
    Servicos/
      Interfaces/
        IAlunosAppServico.cs
      AlunosAppServico.cs
```

### Mapeamentos

O PDF usa `Profiles` do AutoMapper. Neste GitBook, o padrao e Mapster.

```csharp
using Autoglass.Projeto.DataTransfer.Alunos.Requests;
using Autoglass.Projeto.DataTransfer.Alunos.Responses;
using Autoglass.Projeto.Dominio.Alunos.Comandos;
using Autoglass.Projeto.Dominio.Alunos.Entidades;
using Mapster;

namespace Autoglass.Projeto.Aplicacao.Alunos.Mapeamentos
{
    public class AlunosMapsterConfig : IRegister
    {
        public void Register(TypeAdapterConfig config)
        {
            config.NewConfig<AlunoInserirRequest, AlunoInserirComando>();

            config.NewConfig<Aluno, AlunoResponse>()
                .Map(destino => destino.IdUsuario, origem => origem.Usuario.Id)
                .Map(destino => destino.Status, origem => (int)origem.Status);
        }
    }
}
```

Se o projeto legado ainda estiver em AutoMapper, o equivalente conceitual seria:

```csharp
CreateMap<AlunoInserirRequest, AlunoInserirComando>();
CreateMap<Aluno, AlunoResponse>();
```

Mas para novos conteudos deste GitBook, prefira Mapster.

### App service

Interface:

```csharp
using Autoglass.Projeto.DataTransfer.Alunos.Requests;
using Autoglass.Projeto.DataTransfer.Alunos.Responses;

namespace Autoglass.Projeto.Aplicacao.Alunos.Servicos.Interfaces
{
    public interface IAlunosAppServico
    {
        AlunoResponse Inserir(AlunoInserirRequest request);
        AlunoResponse Recuperar(int id);
        IEnumerable<AlunoResponse> Listar();
        void Inativar(int id);
    }
}
```

Implementacao:

```csharp
using Autoglass.Projeto.Aplicacao.Alunos.Servicos.Interfaces;
using Autoglass.Projeto.DataTransfer.Alunos.Requests;
using Autoglass.Projeto.DataTransfer.Alunos.Responses;
using Autoglass.Projeto.Dominio.Alunos.Comandos;
using Autoglass.Projeto.Dominio.Alunos.Entidades;
using Autoglass.Projeto.Dominio.Alunos.Repositorios;
using Autoglass.Projeto.Dominio.Alunos.Servicos.Interfaces;
using Autoglass.Libs.Aplicacao.Transacoes.Interfaces;
using Mapster;
using Microsoft.Extensions.Logging;

namespace Autoglass.Projeto.Aplicacao.Alunos.Servicos
{
    public class AlunosAppServico : IAlunosAppServico
    {
        private readonly IAlunosServico alunosServico;
        private readonly IAlunosRepositorio alunosRepositorio;
        private readonly ILogger<AlunosAppServico> logger;
        private readonly IUnitOfWork unitOfWork;

        public AlunosAppServico(
            IAlunosServico alunosServico,
            IAlunosRepositorio alunosRepositorio,
            ILogger<AlunosAppServico> logger,
            IUnitOfWork unitOfWork)
        {
            this.alunosServico = alunosServico;
            this.alunosRepositorio = alunosRepositorio;
            this.logger = logger;
            this.unitOfWork = unitOfWork;
        }

        public AlunoResponse Inserir(AlunoInserirRequest request)
        {
            AlunoInserirComando comando = request.Adapt<AlunoInserirComando>();

            try
            {
                unitOfWork.BeginTransaction();

                Aluno aluno = alunosServico.Inserir(comando);

                unitOfWork.Commit();

                return aluno.Adapt<AlunoResponse>();
            }
            catch (Exception ex)
            {
                unitOfWork.Rollback();
                logger.LogError(ex, "<{EventId}> | Erro ao <{Acao}> entidade <{Entidade}>",
                    "AlunosAppServico", "Inserir", "Aluno");
                throw;
            }
        }

        public void Inativar(int id)
        {
            try
            {
                unitOfWork.BeginTransaction();

                alunosServico.Inativar(id);

                unitOfWork.Commit();
            }
            catch (Exception ex)
            {
                unitOfWork.Rollback();
                logger.LogError(ex, "<{EventId}> | Erro ao <{Acao}> entidade <{Entidade}>",
                    "AlunosAppServico", "Inativar", "Aluno");
                throw;
            }
        }

        public AlunoResponse Recuperar(int id)
        {
            Aluno aluno = alunosServico.Validar(id);
            return aluno.Adapt<AlunoResponse>();
        }

        public IEnumerable<AlunoResponse> Listar()
        {
            return alunosRepositorio.Query()
                .Select(aluno => new AlunoResponse
                {
                    Id = aluno.Id,
                    IdUsuario = aluno.Usuario.Id,
                    Status = (int)aluno.Status
                });
        }
    }
}
```

Responsabilidades da Aplicacao:

- Receber `Request` da API.
- Converter `Request` para `Comando`.
- Abrir transacao.
- Chamar service de dominio.
- Fazer `Commit` no sucesso e `Rollback` no erro.
- Registrar logs tecnicos.
- Converter entidade ou query para `Response`.
- Orquestrar consultas de leitura quando nao houver regra de negocio pesada.

O que evitar:

- Nao colocar regra de negocio da entidade na Aplicacao.
- Nao chamar controller.
- Nao retornar entidade de dominio para API.
- Nao manter transacao aberta fora do caso de uso.

---

## 05 - Camada de API

A API e a borda HTTP. Ela recebe request, chama a Aplicacao e transforma o resultado em resposta HTTP.

Estrutura:

```text
Autoglass.Projeto.API/
  Controllers/
    Alunos/
      AlunosController.cs
```

Controller:

```csharp
using Autoglass.Projeto.Aplicacao.Alunos.Servicos.Interfaces;
using Autoglass.Projeto.DataTransfer.Alunos.Requests;
using Autoglass.Projeto.DataTransfer.Alunos.Responses;
using Microsoft.AspNetCore.Mvc;

namespace Autoglass.Projeto.API.Controllers.Alunos
{
    [ApiController]
    [Route("api/alunos")]
    public class AlunosController : ControllerBase
    {
        private readonly IAlunosAppServico alunosAppServico;

        public AlunosController(IAlunosAppServico alunosAppServico)
        {
            this.alunosAppServico = alunosAppServico;
        }

        [HttpGet]
        public ActionResult<IEnumerable<AlunoResponse>> Listar()
        {
            return Ok(alunosAppServico.Listar());
        }

        [HttpGet("{id:int}")]
        public ActionResult<AlunoResponse> Recuperar(int id)
        {
            return Ok(alunosAppServico.Recuperar(id));
        }

        [HttpPost]
        public ActionResult<AlunoResponse> Inserir([FromBody] AlunoInserirRequest request)
        {
            AlunoResponse response = alunosAppServico.Inserir(request);
            return CreatedAtAction(nameof(Recuperar), new { id = response.Id }, response);
        }

        [HttpPatch("{id:int}/inativar")]
        public IActionResult Inativar(int id)
        {
            alunosAppServico.Inativar(id);
            return NoContent();
        }
    }
}
```

Regras:

- Controller nao instancia entidade.
- Controller nao chama repositorio.
- Controller nao abre transacao.
- Controller nao contem regra de negocio.
- Controller pode validar detalhes de HTTP, rota, model state e status code.
- Tratamento padronizado de excecoes deve ficar em middleware/filtro global.

---

## 06 - Camada de CrossCutting

CrossCutting concentra recursos transversais que varias camadas precisam usar, mas que nao pertencem a uma entidade especifica.

Exemplos:

- Logging com Serilog ou `ILogger`.
- Tratamento global de excecoes.
- Classes base de excecao.
- Observabilidade e correlation id.
- Resiliencia com Polly.
- Mensageria Kafka, quando for infraestrutura compartilhada.
- Unit of Work e controle de transacao, quando estiverem em biblioteca comum.
- Extensoes reutilizaveis.

Estrutura possivel:

```text
Autoglass.Projeto.CrossCutting/
  Logs/
  Excecoes/
  Middlewares/
  Observabilidade/
  Resiliencia/
```

Regras:

- Nao coloque regra de negocio em CrossCutting.
- Nao coloque entidade de dominio em CrossCutting.
- Nao use CrossCutting como pasta generica para qualquer coisa.
- Se o codigo pertence a um agregado especifico, ele provavelmente nao e transversal.

---

## 07 - Camada de IoC

IoC faz a composicao da aplicacao. E aqui que interfaces sao conectadas a implementacoes concretas.

Estrutura:

```text
Autoglass.Projeto.IoC/
  NativeInjectorBootStrapper.cs
  Modulos/
    DominioModulo.cs
    InfraModulo.cs
    AplicacaoModulo.cs
```

Exemplo simples:

```csharp
using Autoglass.Projeto.Aplicacao.Alunos.Servicos;
using Autoglass.Projeto.Aplicacao.Alunos.Servicos.Interfaces;
using Autoglass.Projeto.Dominio.Alunos.Repositorios;
using Autoglass.Projeto.Dominio.Alunos.Servicos;
using Autoglass.Projeto.Dominio.Alunos.Servicos.Interfaces;
using Autoglass.Projeto.Infra.Alunos.Repositorios;
using Microsoft.Extensions.DependencyInjection;

namespace Autoglass.Projeto.IoC
{
    public static class NativeInjectorBootStrapper
    {
        public static IServiceCollection RegistrarDependencias(this IServiceCollection services)
        {
            services.AddScoped<IAlunosServico, AlunosServico>();
            services.AddScoped<IAlunosRepositorio, AlunosRepositorio>();
            services.AddScoped<IAlunosAppServico, AlunosAppServico>();

            return services;
        }
    }
}
```

Tambem devem ser registrados:

- `ISession` do NHibernate.
- `IUnitOfWork`.
- Configuracoes Mapster.
- Repositorios concretos.
- Services de dominio.
- App services.
- Middlewares e componentes transversais.
- Registradores automaticos com Scrutor, quando o projeto permitir.

Com Scrutor, a ideia e reduzir repeticao:

```csharp
services.Scan(scan => scan
    .FromAssembliesOf(typeof(AlunosServico), typeof(AlunosRepositorio), typeof(AlunosAppServico))
    .AddClasses()
    .AsImplementedInterfaces()
    .WithScopedLifetime());
```

Use registro automatico com criterio. Evite registrar classes por acidente so porque estao no assembly.

---

## 08 - Camada de Jobs

Jobs sao processos fora do ciclo HTTP. Eles podem ser agendados, consumidores de fila, reprocessamentos ou integracoes.

Estrutura:

```text
Autoglass.Projeto.Jobs/
  Alunos/
    InativarAlunosSemVinculoJob.cs
  Consumers/
  Agendamentos/
```

Regras:

- Job nao chama controller.
- Job deve chamar App Service quando representa um caso de uso completo.
- Job pode chamar service de dominio quando estiver dentro de um fluxo interno bem definido.
- Job precisa de log, idempotencia e tratamento de erro.
- Job deve respeitar transacao do caso de uso.
- Job nao deve duplicar regra de negocio da Aplicacao ou do Dominio.

Exemplo:

```csharp
public class InativarAlunosSemVinculoJob
{
    private readonly IAlunosAppServico alunosAppServico;
    private readonly ILogger<InativarAlunosSemVinculoJob> logger;

    public InativarAlunosSemVinculoJob(
        IAlunosAppServico alunosAppServico,
        ILogger<InativarAlunosSemVinculoJob> logger)
    {
        this.alunosAppServico = alunosAppServico;
        this.logger = logger;
    }

    public void Executar(int idAluno)
    {
        logger.LogInformation("Iniciando inativacao do aluno {IdAluno}", idAluno);
        alunosAppServico.Inativar(idAluno);
    }
}
```

---

## 09 - Camada de Tests

Tests garantem que regras e fluxos continuem funcionando.

Estrutura:

```text
Autoglass.Projeto.Tests/
  Dominio/
    Alunos/
      AlunoTests.cs
      AlunosServicoTests.cs
  Aplicacao/
    Alunos/
      AlunosAppServicoTests.cs
  Infra/
    Alunos/
      AlunosMapTests.cs
      AlunosRepositorioTests.cs
  API/
    AlunosControllerTests.cs
```

Prioridade:

1. Entidades e regras de dominio.
2. Services de dominio.
3. App services com transacao, log e mapeamento.
4. Repositorios e mapeamentos criticos.
5. Controllers e contrato HTTP.

Ferramentas:

- xUnit.
- FluentAssertions.
- NSubstitute.
- NBuilder.

Exemplo de teste de entidade:

```csharp
public class AlunoTests
{
    [Fact]
    public void Construtor_QuandoUsuarioValido_DeveCriarAlunoAtivo()
    {
        Usuario usuario = Builder<Usuario>.CreateNew().Build();

        Aluno aluno = new Aluno(usuario);

        aluno.Usuario.Should().Be(usuario);
        aluno.Status.Should().Be(AtivoInativoEnum.Ativo);
    }
}
```

Exemplo de teste de service:

```csharp
public class AlunosServicoTests
{
    private readonly IAlunosRepositorio alunosRepositorio;
    private readonly IUsuariosServico usuariosServico;
    private readonly AlunosServico sut;

    public AlunosServicoTests()
    {
        alunosRepositorio = Substitute.For<IAlunosRepositorio>();
        usuariosServico = Substitute.For<IUsuariosServico>();
        sut = new AlunosServico(alunosRepositorio, usuariosServico);
    }

    [Fact]
    public void Validar_QuandoAlunoNaoExiste_DeveLancarExcecao()
    {
        alunosRepositorio.Recuperar(1).Returns((Aluno)null);

        Action act = () => sut.Validar(1);

        act.Should().Throw<RegraDeNegocioExcecao>()
            .WithMessage("Aluno nao localizado");
    }
}
```

---

## Estrutura final recomendada

```text
src/
  Autoglass.Projeto.Dominio/
    Alunos/
      Entidades/
      Enumeradores/
      Repositorios/
      Comandos/
      Servicos/
        Interfaces/
    Utils/
      Enumeradores/

  Autoglass.Projeto.Infra/
    Alunos/
      Mapeamentos/
      Repositorios/
      Consultas/

  Autoglass.Projeto.DataTransfer/
    Alunos/
      Requests/
      Responses/
      Queries/
      Filtros/

  Autoglass.Projeto.Aplicacao/
    Alunos/
      Mapeamentos/
      Servicos/
        Interfaces/

  Autoglass.Projeto.CrossCutting/
    Logs/
    Excecoes/
    Middlewares/
    Observabilidade/
    Resiliencia/

  Autoglass.Projeto.IoC/
    NativeInjectorBootStrapper.cs
    Modulos/

  Autoglass.Projeto.API/
    Controllers/
      Alunos/

  Autoglass.Projeto.Jobs/
    Alunos/
    Consumers/
    Agendamentos/

tests/
  Autoglass.Projeto.Tests/
    Dominio/
    Aplicacao/
    Infra/
    API/
```

## Checklist de uma nova entidade

1. Criar entidade em `Dominio/<EntidadePlural>/Entidades`.
2. Criar enumeradores especificos, se existirem.
3. Criar comandos da entidade em `Dominio/<EntidadePlural>/Comandos`.
4. Criar interface de repositorio em `Dominio/<EntidadePlural>/Repositorios`.
5. Criar interface e classe de service em `Dominio/<EntidadePlural>/Servicos`.
6. Criar `ClassMap` em `Infra/<EntidadePlural>/Mapeamentos`.
7. Criar repositorio concreto em `Infra/<EntidadePlural>/Repositorios`.
8. Criar requests e responses em `DataTransfer/<EntidadePlural>`.
9. Criar configuracao Mapster em `Aplicacao/<EntidadePlural>/Mapeamentos`.
10. Criar interface e classe de app service em `Aplicacao/<EntidadePlural>/Servicos`.
11. Registrar dependencias na IoC.
12. Criar controller na API.
13. Criar testes de dominio, aplicacao e API.

## Bibliotecas autorizadas

| Categoria | Biblioteca |
| --- | --- |
| Logging | Serilog, `ILogger` |
| ORM | NHibernate, Dapper |
| Testes | xUnit, FluentAssertions, NSubstitute, NBuilder |
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

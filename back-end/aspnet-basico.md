# ASP.NET Básico (Controllers e Services)

## Versão de referência

Conteúdo alinhado ao **ASP.NET Core 10** (ecossistema .NET 10).

## O papel de cada camada

- **Controller**: recebe requisições HTTP, valida entrada, delega regra de negócio.
- **Service**: concentra regras de negócio e orquestra acesso a dados.

## Fluxo básico

1. Cliente chama `GET /produtos`.
2. Controller chama `ProdutoService`.
3. Service busca dados e devolve DTO.
4. Controller retorna `200 OK`.

## Exemplo simplificado

```csharp
[ApiController]
[Route("api/produtos")]
public class ProdutosController : ControllerBase
{
    private readonly ProdutoService _service;

    public ProdutosController(ProdutoService service)
    {
        _service = service;
    }

    [HttpGet]
    public ActionResult<IEnumerable<ProdutoDto>> Listar()
    {
        return Ok(_service.Listar());
    }
}
```

```csharp
public class ProdutoService
{
    public IEnumerable<ProdutoDto> Listar()
    {
        return new List<ProdutoDto>
        {
            new ProdutoDto(1, "Teclado", 199.90m)
        };
    }
}
```

## Boas práticas iniciais

- Controller fino, service com regra.
- DTO para entrada e saída.
- Injeção de dependência para services.
- Para cenários simples, considere **Minimal APIs**; para APIs maiores, mantenha Controllers + Services.

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

### Explicação do exemplo

1. O Controller recebe a requisição HTTP.
2. Ele delega para o Service, sem regra de negócio pesada.
3. O Service monta os dados e retorna DTO.
4. O Controller transforma isso em resposta HTTP (`Ok`).

## Boas práticas iniciais

- Controller fino, service com regra.
- DTO para entrada e saída.
- Injeção de dependência para services.
- Para cenários simples, considere **Minimal APIs**; para APIs maiores, mantenha Controllers + Services.

## Exemplo de requisição e resposta

### Request

```http
POST /api/produtos
Content-Type: application/json

{
  "nome": "Mouse Gamer",
  "preco": 249.90
}
```

### Response

```json
{
  "id": 2,
  "nome": "Mouse Gamer",
  "preco": 249.90
}
```

### Resultado esperado

- Se o payload for válido, a API retorna `200/201` com o produto criado.
- Se o payload for inválido (ex.: nome vazio), deve retornar `400` com erro de validação.

## Erros comuns

- Colocar regra de negócio direto no Controller.
- Expor entidade de domínio diretamente na API em vez de DTO.
- Não validar entrada antes de chamar o serviço.

# DDD (Domain-Driven Design)

DDD é uma forma de modelar software colocando o **domínio do negócio** no centro da aplicação.

## Ideias principais

- **Linguagem ubíqua**: time técnico e negócio usam os mesmos termos.
- **Entidade**: objeto com identidade (ex.: `Pedido` com `Id`).
- **Value Object**: objeto sem identidade, definido por valor (ex.: `Endereco`, `Cpf`).
- **Agregado**: conjunto de objetos consistentes com uma raiz (ex.: `Pedido` + `Itens`).
- **Repositório**: abstrai persistência de agregados.
- **Serviço de domínio**: regra de negócio que não pertence naturalmente a uma única entidade.

## Exemplo de modelagem (e-commerce)

- Entidade raiz: `Pedido`
- Entidades internas: `ItemPedido`
- Value Objects: `Dinheiro`, `EnderecoEntrega`
- Regra: um pedido só pode ser confirmado se tiver ao menos 1 item.

### Leitura prática

- `Pedido` é o agregado raiz: alterações passam por ele.
- `ItemPedido` não deve ser alterado “por fora” do agregado.
- Isso protege consistência de regra.

## Exemplo de entidade com regra de negócio

```csharp
public class Pedido
{
    public Guid Id { get; private set; }
    public List<ItemPedido> Itens { get; private set; } = new();
    public bool Confirmado { get; private set; }

    public Pedido(Guid id)
    {
        Id = id;
    }

    public void AdicionarItem(Guid produtoId, int quantidade, decimal valorUnitario)
    {
        if (quantidade <= 0)
            throw new InvalidOperationException("Quantidade deve ser maior que zero.");

        Itens.Add(new ItemPedido(produtoId, quantidade, valorUnitario));
    }

    public void Confirmar()
    {
        if (Itens.Count == 0)
            throw new InvalidOperationException("Pedido sem itens não pode ser confirmado.");

        Confirmado = true;
    }
}
```

### O que esse código protege

- Não permite item com quantidade inválida.
- Não permite confirmar pedido vazio.
- Mantém regra no domínio, e não no Controller.

## Exemplo de serviço de domínio

Use serviço de domínio quando a regra envolver múltiplos agregados ou política externa.

```csharp
public class PoliticaDeFreteService
{
    public decimal CalcularFrete(decimal subtotal, string ufDestino)
    {
        if (subtotal >= 500) return 0;
        return ufDestino == "ES" ? 20 : 35;
    }
}
```

### Quando é serviço de domínio mesmo

Quando a regra não cabe naturalmente em uma única entidade (ex.: política de frete, antifraude, cálculo tributário externo).

## Exemplo de organização de pastas (DDD)

```text
src/
  Domain/
    Pedidos/
      Pedido.cs
      ItemPedido.cs
      IPedidosRepository.cs
      PoliticaDeFreteService.cs
  Application/
    Pedidos/
      ConfirmarPedidoCommand.cs
      ConfirmarPedidoHandler.cs
  Infrastructure/
    Persistence/
      PedidosRepository.cs
  Api/
    Controllers/
      PedidosController.cs
```

## Quando DDD vale mais a pena

- Domínio complexo, com muitas regras e exceções.
- Necessidade de manter regra de negócio consistente no longo prazo.
- Equipes que precisam alinhar vocabulário entre produto e tecnologia.

## Erros comuns

- Usar DDD só como “nome de pasta”, sem regras no domínio.
- Deixar entidade anêmica (somente `get/set`, sem comportamento).
- Misturar detalhes de infraestrutura dentro de entidades.

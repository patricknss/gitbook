# Fundamentos de Kafka

## Contexto

Notas práticas de Kafka para integração orientada a eventos.

## Versão de referência

Conteúdo alinhado ao ecossistema **Apache Kafka 4.3.0** (release suportada em jun/2026).

Apache Kafka é uma plataforma distribuída de streaming de eventos.

## Conceitos essenciais

- **Producer**: publica mensagens.
- **Topic**: canal lógico de eventos.
- **Partition**: divisão do topic para escalar.
- **Consumer Group**: conjunto de consumidores para processar em paralelo.

## Quando usar

- Processamento de eventos em tempo real.
- Integração entre sistemas com alto volume.
- Pipeline de dados e auditoria de eventos.

## Exemplo de evento

```json
{
  "eventType": "pedido_criado",
  "pedidoId": 1234,
  "clienteId": 88,
  "timestamp": "2026-06-24T12:00:00Z"
}
```

Produtor publica no tópico `pedidos`; consumidores (faturamento, estoque, analytics) processam de forma independente.

## Exemplo prático (Producer e Consumer)

### Producer (C# com Confluent.Kafka)

```csharp
using Confluent.Kafka;

var config = new ProducerConfig { BootstrapServers = "localhost:9092" };
using var producer = new ProducerBuilder<string, string>(config).Build();

await producer.ProduceAsync("pedidos", new Message<string, string>
{
    Key = "pedido-1234",
    Value = "{\"eventType\":\"pedido_criado\",\"pedidoId\":1234}"
});
```

### Consumer (C# com Confluent.Kafka)

```csharp
using Confluent.Kafka;

var config = new ConsumerConfig
{
    BootstrapServers = "localhost:9092",
    GroupId = "faturamento",
    AutoOffsetReset = AutoOffsetReset.Earliest
};

using var consumer = new ConsumerBuilder<string, string>(config).Build();
consumer.Subscribe("pedidos");

var message = consumer.Consume(CancellationToken.None);
Console.WriteLine(message.Message.Value);
```

### Explicação do fluxo

1. Producer publica em `pedidos`.
2. Broker persiste no tópico/partição.
3. Consumer Group lê por offset.
4. Cada consumidor do grupo processa parte da carga.

## Resultado esperado

- Escala horizontal de consumo por partição.
- Reprocessamento possível via controle de offset.

## Erros comuns

- Criar poucas partições e limitar escala.
- Não tratar idempotência no consumidor.
- Ignorar estratégia de retry/DLQ para mensagens inválidas.

## Referências

- [Useful Links](../../useful-links.md)

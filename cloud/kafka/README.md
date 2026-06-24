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

## Referências

- [Useful Links](../../useful-links.md)

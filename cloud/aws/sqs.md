# Amazon SQS

Amazon SQS é um serviço de filas para desacoplar produtores e consumidores.

## Quando usar

- Processamento assíncrono.
- Controle de picos de carga.
- Integração entre microsserviços.

## Conceitos

- **Fila Standard**: maior throughput, ordem não garantida.
- **Fila FIFO**: ordem garantida e sem duplicidade por grupo.
- **DLQ (Dead Letter Queue)**: mensagens com falha após várias tentativas.

## Exemplo de processamento

1. Serviço de pedidos publica mensagem: `pedido_criado`.
2. Worker consome da fila e gera nota fiscal.
3. Se falhar várias vezes, mensagem vai para DLQ para análise.

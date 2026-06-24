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

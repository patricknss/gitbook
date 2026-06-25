# Jobs e Mensageria

Padrao para processos assincronos e consumidores de fila.

## Quando usar job

- Rotina agendada.
- Reprocessamento de dados.
- Integracao que não precisa resposta sincrona.

## Regras essenciais

1. Job não chama controller.
2. Job chama Aplicacao (ou service de dominio com critério).
3. Operacao deve ser idempotente.
4. Falha deve ser logada com contexto e correlation-id.

## Retry e falhas

- Retry com backoff para erro transiente.
- Limite máximo de tentativas.
- Encaminhar para dead-letter quando exceder.

## Contrato de mensagem

- Definir payload versionado.
- Incluir identificador de evento e timestamp.
- Evitar breaking change sem estrategia de versão.

## Checklist para novo consumidor

1. Contrato de mensagem definido.
2. Idempotencia implementada.
3. Retry/backoff configurado.
4. Dead-letter configurada.
5. Logs e metricas publicados.

## Veja tambem

- [Fundamentos de Kafka](../../cloud/kafka/README.md)
- [Observabilidade e Logs](observabilidade-e-logs.md)
- [Arquitetura DDD Autoglass](../ddd/arquitetura-autoglass-ddd.md)

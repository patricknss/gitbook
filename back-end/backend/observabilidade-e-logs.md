# Observabilidade e Logs

Padrao minimo para rastrear problemas e comportamento em producao.

## Objetivos

- Entender falha rapidamente.
- Correlacionar requisicoes ponta a ponta.
- Reduzir tempo de diagnostico.

## Estrutura de log recomendada

| Campo | Exemplo |
| --- | --- |
| timestamp | `2026-06-25T10:30:00Z` |
| level | `Information`, `Error` |
| message | `Erro ao inserir aluno` |
| correlationId | `b9d9...` |
| context | `AlunosAppServico.Inserir` |
| exception | stack trace (apenas log interno) |

## Niveis de log

- `Information`: fluxo normal relevante.
- `Warning`: comportamento anormal recuperavel.
- `Error`: falha de operacao.
- `Debug/Trace`: uso controlado em diagnostico.

## O que nao logar

- Senhas, tokens, dados sensiveis.
- Payload completo sem necessidade.
- Dados pessoais sem mascaramento.

## Correlation-id

- Receber de header externo ou gerar internamente.
- Propagar para logs e chamadas downstream.
- Incluir em respostas de erro quando aplicavel.

## Alertas minimos

1. Taxa de erro por endpoint.
2. Tempo medio e p95 de resposta.
3. Falhas de integracao externa.
4. Jobs com repeticao de falha.

## Veja tambem

- [Playbook de API](playbook-api.md)
- [Seguranca no Backend](seguranca-backend.md)
- [Arquitetura DDD Autoglass](../ddd/arquitetura-autoglass-ddd.md)

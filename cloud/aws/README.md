# Visão geral AWS para devs

## Contexto

Notas práticas de serviços AWS usados com frequência em aplicações backend.

## O que está documentado

Cobrir conceitos iniciais de serviços muito usados em aplicações modernas:

1. Cognito
2. S3
3. SQS

Cada página foca no que é, quando usar e exemplo prático.

## Exemplos rápidos

### 1. Login com Cognito

Fluxo típico:

1. Frontend autentica no User Pool.
2. Recebe `id_token` e `access_token`.
3. API valida JWT para autorizar acesso.

### 2. Upload de imagem com S3

Fluxo típico:

1. API gera URL pré-assinada.
2. Frontend envia arquivo direto para o S3.
3. API salva apenas a chave do arquivo no banco.

### 3. Processamento assíncrono com SQS

Fluxo típico:

1. Serviço A publica mensagem `pedido_criado`.
2. Worker consome e processa.
3. Em falha recorrente, mensagem vai para DLQ.

## Nota de atualização

Serviços AWS são gerenciados e não seguem versão semântica de framework como Angular/.NET.
Este conteúdo foi revisado em **jun/2026** com foco em conceitos atuais de uso.

## Referências

- [Useful Links](../../useful-links.md)

# Playbook de API (.NET)

Guia pratico para padronizar endpoints HTTP em APIs ASP.NET.

## Contrato de endpoint

- URL no plural e em minusculo: `/api/clientes`.
- Verbos corretos: `GET`, `POST`, `PUT/PATCH`, `DELETE`.
- Response sempre via DTO, nunca entidade de dominio.

## Status code padrão

| Cenario | Status |
| --- | --- |
| Consulta com sucesso | `200 OK` |
| Criacao com sucesso | `201 Created` |
| Sem conteúdo | `204 No Content` |
| Requisicao invalida | `400 Bad Request` |
| Não autenticado | `401 Unauthorized` |
| Sem permissao | `403 Forbidden` |
| Não encontrado | `404 Not Found` |
| Conflito de regra | `409 Conflict` |
| Erro inesperado | `500 Internal Server Error` |

## Paginacao e filtros

- Parametros recomendados: `pagina`, `quantidade`, `ordenarPor`, `tipoOrdenacao`.
- Restringir tamanho máximo de pagina para evitar abuso.
- Responder metadados de pagina quando aplicavel.

## Idempotencia

- `GET`, `PUT`, `DELETE` devem ser idempotentes.
- Em `POST` sensivel (ex.: pagamento), considerar `Idempotency-Key`.

## Validacao de entrada

- Validar shape e formato na borda (API).
- Validacao de regra de negócio deve ficar no Dominio.
- Retornar erro padronizado (codigo + mensagem + detalhes).

## Tratamento de erro

- Middleware/filtro global para excecoes.
- Não expor stack trace em produção.
- Logar erro com correlation-id.

## Seguranca básica

1. Autenticacao obrigatoria para rotas privadas.
2. Autorizacao por perfil/escopo.
3. Nunca confiar em dados vindos do cliente.
4. Sanitizar e validar entrada.

## Exemplo de checklist para endpoint novo

1. DTO de entrada e saida criado.
2. Status code e cenarios de erro definidos.
3. Validacao de entrada implementada.
4. Log e correlacao cobrindo falhas.
5. Teste de contrato da rota criado.

## Veja tambem

- [ASP.NET Basico](aspnet-basico.md)
- [Arquitetura DDD Autoglass](../ddd/arquitetura-autoglass-ddd.md)
- [Seguranca no Backend](seguranca-backend.md)

# Seguranca no Backend

Checklist minimo de seguranca para APIs e persistencia.

## Entrada e validacao

- Validar tamanho, formato e faixa de valores.
- Bloquear dados inesperados no payload.
- Sanitizar campos usados em busca/filtro.

## Autenticacao e autorizacao

- Exigir autenticacao em rotas privadas.
- Aplicar autorizacao por papel/escopo.
- Nao confiar em permissao vinda do cliente.

## Dados sensiveis

- Nao logar segredos e credenciais.
- Usar mascaramento quando necessario.
- Armazenar segredo fora do codigo (variavel de ambiente/cofre).

## Banco de dados

- Sempre usar parametros em consultas SQL.
- Evitar concatenacao de SQL com input do usuario.
- Garantir principle of least privilege no usuario de banco.

## API hardening

1. Limitar tamanho de request.
2. Aplicar rate limit em rotas criticas.
3. Definir timeout de chamadas externas.
4. Padronizar retorno de erro sem detalhes internos.

## LGPD e compliance

- Coletar apenas dado necessario.
- Definir politica de retencao.
- Registrar acesso/auditoria quando exigido.

## Veja tambem

- [Playbook de API](playbook-api.md)
- [Observabilidade e Logs](observabilidade-e-logs.md)
- [Persistencia: NHibernate x Dapper](persistencia-nhibernate-dapper.md)

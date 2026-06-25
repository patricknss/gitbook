# Liquibase Operacional (Padrao de Time)

Guia operacional completo para uso de Liquibase no dia a dia.

## Estrutura recomendada

```text
{schema}/
  changelogs/
    {schema}db.changelog-0.1.0.xml
    {schema}db.changelog-0.2.0.xml
  scripts/
    0.2.0/
      updates/
      rollbacks/
  liquibase.properties
  liquibase.hml.properties
  liquibase.prod.properties
  {schema}db.changelog-master.xml
```

## Regras de changeset

- ID unico por card: `{CARD}_{sequencial}`.
- Um script por changeset.
- Rollback obrigatorio.
- Ordem importa e deve refletir dependencias.

## Nomenclatura de script

`{NN}-SRC_{CARD}_{DESCRICAO}.sql`  
`{NN}-SRC_{CARD}_{DESCRICAO}-ROLLBACK.sql`

Exemplo:

- `01-SRC_BO-17474_CRIA_TABELA_CLIENTES.sql`
- `01-SRC_BO-17474_CRIA_TABELA_CLIENTES-ROLLBACK.sql`

## Comandos essenciais

```bash
liquibase --defaultsFile=liquibase.properties validate
liquibase --defaultsFile=liquibase.properties status
liquibase --defaultsFile=liquibase.properties update
liquibase --defaultsFile=liquibase.properties rollback 0.2.0
liquibase --defaultsFile=liquibase.properties rollbackCount 1
liquibase --defaultsFile=liquibase.properties updateSQL
```

## Quando usar cada comando

- `validate`: validar consistencia de changelog antes de executar.
- `status`: listar pendencias.
- `update`: aplicar mudancas pendentes.
- `rollback {tag}`: voltar ate uma tag.
- `rollbackCount`: desfazer quantidade fixa de changesets.
- `updateSQL`: gerar SQL sem aplicar (dry-run).

## Fluxo para nova alteracao

1. Definir release alvo.
2. Criar script de update.
3. Criar script de rollback.
4. Registrar changeset no changelog da release.
5. Rodar `validate`.
6. Conferir `status`.
7. Executar `update`.

## Fluxo para nova release

1. Criar changelog da versao.
2. Incluir tag de versao como primeiro changeset.
3. Atualizar changelog master com novo include.
4. Adicionar scripts e changesets.

## Matriz de rollback

| Mudanca | Rollback esperado |
| --- | --- |
| `CREATE TABLE` | `DROP TABLE` |
| `CREATE SEQUENCE` | `DROP SEQUENCE` |
| `ALTER TABLE ADD COLUMN` | `ALTER TABLE DROP COLUMN` |
| `INSERT` | `DELETE` com filtro seguro |

## Troubleshooting rapido

1. `validate` falhou: corrigir include/xsd/sintaxe e reexecutar.
2. Lock ativo: investigar processo concorrente antes de qualquer acao manual.
3. Checksum divergente: nao alterar changeset ja aplicado; criar novo changeset.
4. Ordem quebrada: ajustar includes e dependencias de script.

## Veja tambem

- [Liquibase: introducao](liquibase-introducao.md)
- [Banco de Dados](README.md)
- [Runbook Operacional](../../workflow/runbook-operacional.md)

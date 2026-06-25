# Liquibase: introdução

## Versão de referência

Conteúdo alinhado ao **Liquibase Community 5.0.3**.

Liquibase é uma ferramenta de versionamento de banco de dados baseada em changelogs.

## Conceitos-chave

- **Changelog**: arquivo principal com alterações.
- **Changeset**: unidade versionada de mudança.
- **Rollback**: forma de desfazer alterações.

## Exemplo de changeset (XML)

```xml
<databaseChangeLog xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
                   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                   xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
                   http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-latest.xsd">
    <changeSet id="001-create-clientes" author="patrick">
        <createTable tableName="clientes">
            <column name="id" type="INT">
                <constraints primaryKey="true" nullable="false"/>
            </column>
            <column name="nome" type="VARCHAR(100)"/>
        </createTable>
    </changeSet>
</databaseChangeLog>
```

### Explicação do changeset

- `id` + `author` identificam unicamente a mudança.
- Se já executado, o Liquibase não reaplica.
- Isso garante histórico incremental de schema.

## Benefícios

- Histórico rastreável de schema.
- Padronização entre ambientes.
- Execução automatizada no deploy.

## Exemplo de execução no dia a dia

```bash
liquibase validate
liquibase update
liquibase history
liquibase rollbackCount 1
```

### O que cada comando faz

- `validate`: valida a estrutura do changelog antes de executar qualquer alteração.
- `update`: aplica pendências.
- `history`: mostra mudanças já executadas.
- `rollbackCount 1`: desfaz o último changeset aplicado.

## Liquibase validate (quando e por que usar)

`validate` é a etapa de segurança para garantir que os changelogs estão consistentes antes do `update`.

Use principalmente para detectar:

- erro de sintaxe no changelog;
- referência quebrada de include/changelog;
- inconsistências de identificação de changeset (`id`/`author`);
- problemas de estrutura que impediriam a execução.

Fluxo recomendado:

1. Rode `validate`.
2. Corrija o que for apontado.
3. Só depois rode `update`.

Exemplo comum por ambiente:

```bash
liquibase --defaultsFile=liquibase.properties validate
liquibase --defaultsFile=liquibase.properties update
```

## Erros comuns

- Editar changeset já aplicado em produção.
- Não versionar changelog junto com código.
- Subir sem estratégia de rollback.

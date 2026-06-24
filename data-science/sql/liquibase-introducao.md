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

## Benefícios

- Histórico rastreável de schema.
- Padronização entre ambientes.
- Execução automatizada no deploy.

## Exemplo de execução no dia a dia

```bash
liquibase update
liquibase history
liquibase rollbackCount 1
```

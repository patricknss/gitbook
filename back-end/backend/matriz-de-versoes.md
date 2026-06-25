# Matriz de Versoes e Compatibilidade

Versoes-base para reduzir divergencia entre ambientes e times.

## Stack principal (jun/2026)

| Componente | Versao de referencia |
| --- | --- |
| .NET SDK | 10.0.301 |
| .NET Runtime | 10.0 (LTS) |
| C# | 14 |
| ASP.NET Core | 10 |
| Liquibase Community | 5.0.3 |

## Bibliotecas-chave

| Categoria | Padrao |
| --- | --- |
| Mapeamento | Mapster |
| ORM de escrita | NHibernate |
| Consulta SQL direta | Dapper |
| Testes | xUnit + FluentAssertions + NSubstitute |
| Logging | Serilog/ILogger |

## Politica de atualizacao

1. Atualizacao de versão maior exige plano e validação por PR.
2. Atualizacao de patch/minor pode ser continua, com changelog.
3. Não misturar versões conflitantes sem registro explicito.

## Checklist de compatibilidade

1. Projeto compila com SDK de referencia.
2. Pacotes principais sem conflito de dependencia.
3. Pipeline usa versão compativel com local.
4. Documentacao atualizada quando versão mudar.

## Veja tambem

- [Backend (Fundamentos e APIs)](README.md)
- [Playbook de API](playbook-api.md)
- [Liquibase: introducao](../../data-science/sql/liquibase-introducao.md)

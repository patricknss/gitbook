# Guia de Testes Backend (.NET)

Padrao para testes por camada em projetos backend.

## Estrategia por camada

1. **Dominio**: prioridade maxima (entidades e services).
2. **Aplicacao**: fluxo de caso de uso, transacao e mapeamento.
3. **Infra**: consultas/repositorios criticos.
4. **API**: contrato HTTP e codigos de resposta.

## Ferramentas padrao

- xUnit
- FluentAssertions
- NSubstitute
- NBuilder

## Nomenclatura recomendada

- Arquivo: `{Classe}Tests.cs`
- Metodo: `{Metodo}_Quando{Cenario}_Deve{Resultado}`

## Cobertura minima sugerida por alteracao

| Tipo de alteracao | Minimo recomendado |
| --- | --- |
| Regra de dominio | Teste unitario da regra + teste de falha |
| Novo endpoint | Teste de contrato (200/400/404) |
| Nova consulta | Teste de resultado esperado e filtro |
| Fluxo transacional | Teste de commit e rollback |

## Checklist para PR

1. Teste de caminho feliz.
2. Teste de erro/regra violada.
3. Nenhum teste dependente de ordem.
4. Nomes de teste descrevendo comportamento.
5. Sem mock de detalhe que nao pertence ao contrato.

## Veja tambem

- [xUnit](../../bibliotecas/xunit.md)
- [Playbook de API](playbook-api.md)
- [Arquitetura DDD Autoglass](../ddd/arquitetura-autoglass-ddd.md)

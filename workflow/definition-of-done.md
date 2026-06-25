# Definition of Done (DoD)

Criterios minimos para considerar uma entrega concluida.

## DoD geral

1. Escopo funcional implementado.
2. Documentacao atualizada quando relevante.
3. Links internos validos.
4. Codigo revisado por PR.
5. Branch pronta para merge sem pendencias.

## DoD para feature backend

1. Endpoint/servico implementado conforme contrato.
2. Regras de dominio aplicadas na camada correta.
3. Tratamento de erro padronizado.
4. Testes do fluxo principal e falha principal.
5. Logs suficientes para diagnostico.

## DoD para alteracao SQL/Liquibase

1. Script de update criado.
2. Script de rollback correspondente criado.
3. Changeset registrado com `id`/`author` corretos.
4. `validate` previsto no fluxo de execucao.
5. Ordem de aplicação documentada.

## DoD para novo conteúdo no GitBook

1. Estrutura minima (contexto, aplicação, risco/checklist).
2. Referencias cruzadas para paginas relacionadas.
3. Sem links quebrados.
4. Linguagem consistente com o guia editorial.

## Veja tambem

- [Guia Editorial do GitBook](guia-editorial-gitbook.md)
- [Runbook Operacional](runbook-operacional.md)
- [Git Flow](git-flow.md)

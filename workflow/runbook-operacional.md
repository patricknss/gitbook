# Runbook Operacional

Passo a passo curto para preparar alteracoes com seguranca.

## Fluxo basico de trabalho

1. Atualizar branch local.
2. Implementar alteracao.
3. Revisar arquivos e links.
4. Commitar com padrao do card.
5. Subir branch e abrir PR.

## Checklist antes do PR

1. Escopo fechado ao objetivo do card.
2. Sem arquivos temporarios indevidos.
3. Documentacao ajustada quando necessario.
4. Links internos validos.
5. Mensagem de commit no padrao.

## Checklist de PR

1. Titulo claro do que foi alterado.
2. Descricao com motivacao e impacto.
3. Lista objetiva de arquivos principais.
4. Observacoes de risco (se houver).

## Fluxo rapido para docs

1. Criar/editar pagina.
2. Atualizar `SUMMARY.md`.
3. Adicionar referencias cruzadas.
4. Validar links internos.

## Fluxo rapido para SQL/Liquibase

1. Criar scripts update/rollback.
2. Registrar changeset.
3. Rodar `validate`.
4. Somente depois executar `update`.

## Veja tambem

- [Git Flow](git-flow.md)
- [Definition of Done](definition-of-done.md)
- [Guia Editorial do GitBook](guia-editorial-gitbook.md)

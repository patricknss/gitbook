# Git Flow

Guia prático para fluxo completo: desenvolvimento, release, produção, hotfix e rollback.

## Objetivo do fluxo

- Garantir previsibilidade de entrega.
- Separar trabalho em progresso de código pronto para produção.
- Permitir correção urgente sem travar o desenvolvimento.

## Modelo de branches

- `main` (ou `master`): produção.
- `desenvolvimento`: integração contínua das features.
- `feature/*`: implementação de card.
- `release/*`: preparação de versão para subir.
- `hotfix/*`: correção urgente em produção.

## Padrão de nomenclatura de branch

- Feature: `feature/{CARD}-{nome-kebab-case}`
- Bugfix: `bugfix/{CARD}-{nome-kebab-case}`
- Release: `release/{versao}` (ex.: `release/1.4.0`)
- Hotfix: `hotfix/{versao}` (ex.: `hotfix/1.4.1`)

Exemplos:

- `feature/ABC-123-cadastro-de-clientes`
- `bugfix/ABC-456-correcao-validacao-email`
- `release/1.4.0`
- `hotfix/1.4.1`

## Regras importantes

- Usar sempre minúsculas e kebab-case.
- Não commitar direto em `main/master` ou `desenvolvimento` (sempre via PR).
- Branch curta, focada em um objetivo.
- Toda subida para produção precisa de tag de versão.

## Fluxo de feature (dia a dia)

1. Criar branch a partir de `desenvolvimento`.
2. Implementar, abrir PR para `desenvolvimento`.
3. Após aprovação, merge.

```bash
git checkout desenvolvimento
git pull origin desenvolvimento
git checkout -b feature/ABC-123-cadastro-de-clientes
git push -u origin feature/ABC-123-cadastro-de-clientes
```

## Fluxo de release (abertura)

Quando o conjunto da sprint estiver pronto, abrir release a partir de `desenvolvimento`:

```bash
git checkout desenvolvimento
git pull origin desenvolvimento
git checkout -b release/1.4.0
git push -u origin release/1.4.0
```

Nesta fase entram apenas:

- ajustes finais;
- correções de bug da release;
- versionamento/changelog.

## Fluxo de release (fechamento e produção)

Depois de homologar:

1. PR `release/1.4.0` -> `main` (produção).
2. Criar tag da versão.
3. PR `release/1.4.0` -> `desenvolvimento` (back-merge).
4. Excluir branch de release.

```bash
git checkout main
git pull origin main
git merge --no-ff release/1.4.0
git tag -a v1.4.0 -m "Release v1.4.0"
git push origin main --tags
```

## Fluxo de hotfix (produção quebrada)

Quando surgir bug crítico em produção:

1. Criar `hotfix/*` a partir de `main`.
2. Corrigir e validar.
3. Merge em `main` + nova tag.
4. Back-merge em `desenvolvimento`.

```bash
git checkout main
git pull origin main
git checkout -b hotfix/1.4.1
# corrige bug
git checkout main
git merge --no-ff hotfix/1.4.1
git tag -a v1.4.1 -m "Hotfix v1.4.1"
git push origin main --tags
```

## Estrutura recomendada por ambiente

- `desenvolvimento` -> ambiente DEV
- `release/*` -> ambiente HML/QA
- `main` -> ambiente PROD

## Padrão de commit

```text
{CARD} - {Resumo no presente}

{Detalhamento opcional}
```

## Veja tambem

- [Definition of Done](definition-of-done.md)
- [Runbook Operacional](runbook-operacional.md)
- [Guia Editorial do GitBook](guia-editorial-gitbook.md)

Exemplo:

```text
ABC-123 - Adiciona documentação de cache em memória

Inclui exemplos de IMemoryCache, invalidação e boas práticas.
```

## Fluxo de commit e push

```bash
git add .
git commit -m "ABC-123 - Adiciona documentação de cache em memória"
git push
```

## Checklist de subida para produção

- PR da release aprovado.
- Build/testes verdes.
- Homologação concluída.
- Changelog atualizado.
- Tag criada.
- Plano de rollback definido.

## Rollback (estratégia rápida)

Se a release falhar em produção:

1. Reverter commit de merge da release na `main`; ou
2. Reimplantar a tag anterior estável.

Exemplo de reversão:

```bash
git checkout main
git pull origin main
git revert -m 1 <sha-do-merge-da-release>
git push origin main
```

## Boas práticas de PR

- Título objetivo (mesmo padrão do commit principal).
- Descrever contexto, alteração e impacto.
- Se possível, anexar evidências (print, link, teste manual).
- Definir responsável por aprovação de release.

# Estrutura básica de projetos Angular

## Versão de referência

Conteúdo alinhado ao **Angular 22**.

## Pastas comuns

- `src/app`: componentes, serviços e regras da aplicação.
- `src/assets`: imagens e arquivos estáticos.
- `src/environments`: configurações por ambiente.

## Arquivos relevantes

- `main.ts`: bootstrap da aplicação.
- `app.config.ts`: configuração principal da aplicação.
- `app.routes.ts`: rotas da aplicação.

## Organização inicial recomendada

- `core`: serviços singleton (auth, interceptors).
- `shared`: componentes e pipes reutilizáveis.
- `features`: funcionalidades por domínio.

## Exemplo de estrutura

```text
src/
  app/
    core/
      interceptors/
    shared/
      components/
    features/
      produtos/
        produtos.component.ts
        produtos.service.ts
```

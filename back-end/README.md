# Backend

## Contexto

Notas e exemplos práticos do que já foi estudado e aplicado em backend.

## Versões de referência (jun/2026)

- .NET: **10.0 (LTS)** (SDK 10.0.301)
- C#: **14**
- ASP.NET Core: **10**

## O que está documentado

1. Fundamentos de C# (tipos, coleções, exceções).
2. Programação orientada a objetos (classes, herança, interfaces).
3. DDD (Domain-Driven Design): entidades, agregados, serviços de domínio e repositórios.
4. Conceitos básicos de ASP.NET (controllers, services).
5. Async/await e noções básicas de concorrência.
6. Gerenciamento de cache em memória com `IMemoryCache`.

## Exemplo prático

Projeto: **API de catálogo de produtos**.

1. Criar classes `Produto`, `Categoria` e interfaces de serviço.
2. Expor endpoints com ASP.NET (`GET /produtos`, `POST /produtos`).
3. Buscar dados de forma assíncrona com `async/await`.
4. Tratar falhas com exceções de domínio e resposta HTTP adequada.

## Referências

- [Useful Links](../useful-links.md)

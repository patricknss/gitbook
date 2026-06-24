# Async/Await e Concorrência

## Versão de referência

Conteúdo alinhado ao **.NET 10 / C# 14**.

## Conceito rápido

- **Async/await** evita bloqueio de thread em operações I/O (API, banco, disco).
- **Concorrência** é executar tarefas que podem progredir "ao mesmo tempo".

## Exemplo com `Task` e `await`

```csharp
public async Task<string> BuscarDadosAsync(HttpClient httpClient)
{
    var resposta = await httpClient.GetStringAsync("https://api.exemplo.com/dados");
    return resposta;
}
```

## Executando tarefas em paralelo

```csharp
public async Task ProcessarAsync()
{
    Task t1 = Task.Delay(300);
    Task t2 = Task.Delay(500);

    await Task.WhenAll(t1, t2);
}
```

## Cuidados importantes

- Evite `.Result` e `.Wait()` em código assíncrono.
- Prefira `Task.WhenAll` para tarefas independentes.
- Use `CancellationToken` em operações longas.

## Exemplo com cancelamento

```csharp
public async Task<string> ConsultarComCancelamentoAsync(
    HttpClient httpClient,
    CancellationToken cancellationToken)
{
    using var response = await httpClient.GetAsync(
        "https://api.exemplo.com/status",
        cancellationToken);

    response.EnsureSuccessStatusCode();
    return await response.Content.ReadAsStringAsync(cancellationToken);
}
```

# Noções iniciais de RxJS

## Versão de referência

Conteúdo alinhado ao **RxJS 7.8.2**.

RxJS trabalha com fluxos assíncronos via `Observable`.

## Conceitos iniciais

- `Observable`: fonte de dados assíncrona.
- `Observer`: quem recebe os dados.
- `pipe`: encadeia operadores.

## Exemplo

```ts
this.service.listar()
  .pipe(
    map(lista => lista.filter(p => p.ativo)),
    catchError(() => of([]))
  )
  .subscribe(produtos => this.produtos = produtos);
```

## Operadores úteis no começo

- `map`
- `filter`
- `tap`
- `catchError`
- `switchMap`

## Exemplo: busca com debounce

```ts
this.termoBusca.valueChanges.pipe(
  debounceTime(300),
  distinctUntilChanged(),
  switchMap(termo => this.service.buscar(termo))
).subscribe(resultado => this.produtos = resultado);
```

### Explicação do fluxo

1. Usuário digita.
2. `debounceTime` espera pausa de 300ms.
3. `distinctUntilChanged` ignora termo repetido.
4. `switchMap` cancela requisição anterior e usa só a mais recente.

## Erros comuns

- Esquecer de cancelar stream em componentes de longa vida.
- Usar `mergeMap` para busca e receber respostas fora de ordem.
- Tratar erro fora do pipeline e quebrar fluxo.

# Serviços e consumo de APIs

## Versão de referência

Conteúdo alinhado ao **Angular 22** com `HttpClient`.

Serviços centralizam integração e lógica reutilizável.

## Exemplo com `HttpClient`

```ts
@Injectable({ providedIn: "root" })
export class ProdutosService {
  constructor(private http: HttpClient) {}

  listar() {
    return this.http.get<Produto[]>("/api/produtos");
  }
}
```

### Explicação

- Serviço encapsula acesso HTTP.
- Componente só consome método pronto.
- Facilita teste e reaproveitamento.

## Uso no componente

```ts
export class ProdutosComponent {
  produtos: Produto[] = [];

  constructor(private service: ProdutosService) {}

  ngOnInit() {
    this.service.listar().subscribe(dados => this.produtos = dados);
  }
}
```

## Cuidados

- Tratar erro no fluxo da requisição.
- Evitar duplicar chamadas HTTP no componente.

## Exemplo com tratamento de erro

```ts
listar() {
  return this.http.get<Produto[]>("/api/produtos").pipe(
    catchError(() => of([]))
  );
}
```

### Resultado esperado

- Em sucesso: retorna lista de produtos.
- Em falha: retorna lista vazia (fallback controlado).

## Erros comuns

- Fazer chamada HTTP diretamente no componente.
- Ignorar tratamento de erro.
- Duplicar mesma chamada em múltiplos componentes.

# Componentes, templates e data binding

## Versão de referência

Conteúdo alinhado ao **Angular 22**.

## Componente Angular

```ts
@Component({
  selector: "app-contador",
  template: `<p>{{ total }}</p><button (click)="incrementar()">+</button>`
})
export class ContadorComponent {
  total = 0;
  incrementar() { this.total++; }
}
```

## Tipos de binding

- Interpolação: `{{ valor }}`
- Property binding: `[disabled]="carregando"`
- Event binding: `(click)="salvar()"`
- Two-way binding: `[(ngModel)]="filtro"`

## Boas práticas

- Componente com responsabilidade única.
- Template simples e sem regra pesada.
- Para estado local de UI, prefira **signals** quando fizer sentido.

## Exemplo de `@Input` e `@Output`

```ts
@Component({
  selector: "app-filtro",
  template: `<input [value]="valor" (input)="valorChange.emit($any($event.target).value)" />`
})
export class FiltroComponent {
  @Input() valor = "";
  @Output() valorChange = new EventEmitter<string>();
}
```

# Fundamentos de TypeScript

## Versão de referência

Conteúdo alinhado ao **TypeScript 6.0.3**.

TypeScript adiciona tipagem estática ao JavaScript.

## Tipagem básica

```ts
let nome: string = "Patrick";
let idade: number = 28;
let ativo: boolean = true;
```

## Interfaces

```ts
interface Usuario {
  id: number;
  nome: string;
  email?: string;
}
```

## Classes

```ts
class Pessoa {
  constructor(public nome: string) {}
}
```

## Ganhos principais

- Menos erros em runtime.
- Melhor autocomplete e refatoração.

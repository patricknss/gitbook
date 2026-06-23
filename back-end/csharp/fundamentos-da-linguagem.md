# Fundamentos da Linguagem

## Sintaxe Básica

```csharp
using System; // Importa funcionalidades do .NET

class Program
{
    static void Main(string[] args) // Ponto de entrada do programa
    {
        Console.WriteLine("Eaí pessoal tranquilidade pura?");
    }
}
```

#### Observações

`using System;` → importa funcionalidades básicas como `Console`.\
`class Program` → define uma classe chamada `Program`.\
`static void Main(string[] args)` → método principal, onde a execução começa.\
`Console.WriteLine(...)` → imprime texto no console.

***

## Tipos de Dados

Primitivos

```csharp
int idade = 25;
float altura = 1.75f;
double peso = 70.5;
bool ativo = true;
char letra = 'A';
string nome = "Patrick";
```

Tipo por referência

```csharp
class Pessoa
{
    public string Nome;
}

Pessoa p = new Pessoa();
p.Nome = "Ana";
```

> Objetos criados com `class` são armazenados no **heap** e acessados por **referência**.

Tipo anônimo

```csharp
var aluno = new { Nome = "Lucas", Nota = 9.5 };
Console.WriteLine(aluno.Nome); // Lucas
```

Tipo dinâmico (`dynamic`)

```csharp
dynamic variavel = 10;
variavel = "Agora sou string!";
Console.WriteLine(variavel);
```

> Perigoso em produção: sem verificação de tipo em tempo de compilação.

***

## Operadores

#### Aritméticos

```csharp
int soma = 2 + 3;     // 5
int produto = 4 * 2;  // 8
```

Relacionais

```csharp
bool maior = 10 > 5;       // true
bool igual = "a" == "b";   // false
```

Lógicos

```csharp
bool cond = (5 > 3) && (2 < 4); // true
bool neg = !(true);            // false
```

Ternário

```csharp
int idade = 18;
string status = (idade >= 18) ? "Adulto" : "Menor";
```

Null-coalescing (`??`)

```csharp
string nome = null;
string resultado = nome ?? "Desconhecido"; // retorna "Desconhecido"
```

***

## Controles de Fluxo

#### Condicionais

```csharp
if (idade >= 18)
{
    Console.WriteLine("Maior de idade");
}
else
{
    Console.WriteLine("Menor de idade");
}
```

#### >`switch`

```csharp
int opcao = 2;

switch (opcao)
{
    case 1:
        Console.WriteLine("Opção 1");
        break;
    case 2:
        Console.WriteLine("Opção 2");
        break;
    default:
        Console.WriteLine("Opção inválida");
        break;
}
```

Loops

```csharp
// while
int i = 0;
while (i < 3)
{
    Console.WriteLine(i);
    i++;
}

// do-while
int j = 0;
do
{
    Console.WriteLine(j);
    j++;
} while (j < 3);

// for
for (int k = 0; k < 3; k++)
{
    Console.WriteLine(k);
}

// foreach
int[] numeros = { 1, 2, 3 };
foreach (int n in numeros)
{
    Console.WriteLine(n);
}
```

`> break` e `continue`

```csharp
for (int x = 0; x < 5; x++)
{
    if (x == 2) continue; // pula o 2
    if (x == 4) break;    // interrompe o loop
    Console.WriteLine(x);
}
```

***

## **Métodos e Parâmetros**

#### Método básico

```csharp
void Saudacao(string nome)
{
    Console.WriteLine($"Olá, {nome}");
}
```

`ref` (passa referência, exige inicialização)

```csharp
void Dobrar(ref int numero)
{
    numero *= 2;
}

int n = 5;
Dobrar(ref n); // n agora é 10
```

`out` (sem necessidade de inicialização)

```csharp
void GerarNumero(out int x)
{
    x = 42;
}
int valor;
GerarNumero(out valor);
```

`params` (quantidade variável de argumentos)

```csharp
void Somar(params int[] numeros)
{
    int total = 0;
    foreach (int n in numeros) total += n;
    Console.WriteLine(total);
}
Somar(1, 2, 3); // 6
```

Parâmetros opcionais

```csharp
void Mostrar(string msg = "Olá")
{
    Console.WriteLine(msg);
}
Mostrar();       // Olá
Mostrar("Oi!");  // Oi!
```

***

## **Escopo e Visibilidade**

#### "Escopo"

Refere-se ao **alcance onde uma variável é válida**:

```csharp
void Teste()
{
    int local = 10; // escopo local
    if (true)
    {
        int interno = 5; // escopo mais interno
        Console.WriteLine(interno);
    }
    // Console.WriteLine(interno); ❌ erro: fora do escopo
}
```

#### Modificadores de visibilidade (acesso)

| Modificador          | Visível para                           |
| -------------------- | -------------------------------------- |
| `public`             | Em qualquer lugar                      |
| `private`            | Somente dentro da própria classe       |
| `protected`          | Na própria classe e classes derivadas  |
| `internal`           | Dentro do mesmo assembly/projeto       |
| `protected internal` | Em classes derivadas ou mesmo assembly |

```csharp
public class Exemplo
{
    private int segredo = 42;
    public int publico = 100;
}
```

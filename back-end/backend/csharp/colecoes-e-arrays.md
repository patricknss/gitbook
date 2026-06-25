# Coleções e Arrays

### **Arrays**

#### Array unidimensional

```csharp
int[] numeros = new int[3];
numeros[0] = 10;
numeros[1] = 20;
numeros[2] = 30;

foreach (int n in numeros)
{
    Console.WriteLine(n);
}
```

#### Array com inicialização direta

```csharp
string[] frutas = { "Maçã", "Banana", "Laranja" };
Console.WriteLine(frutas[1]); // Banana
```

#### Array multidimensional (matriz)

```csharp
int[,] matriz = new int[2, 2] { {1, 2}, {3, 4} };

Console.WriteLine(matriz[0, 1]); // 2
```

#### Classe `Array` (base de todos os arrays)

```csharp
int[] valores = { 5, 2, 8, 1 };
Array.Sort(valores); // Ordena
Array.Reverse(valores); // Inverte

foreach (int v in valores)
{
    Console.WriteLine(v);
}
```

***

### **Listas (`List<T>`)**

Lista genérica que pode crescer dinamicamente.

```csharp
using System.Collections.Generic;

List<string> nomes = new List<string>();
nomes.Add("Ana");
nomes.Add("Lucas");

Console.WriteLine(nomes[0]); // Ana

foreach (string nome in nomes)
{
    Console.WriteLine(nome);
}
```

#### Métodos úteis:

```csharp
csharpCopiarEditarnomes.Remove("Ana");
bool existe = nomes.Contains("Lucas");
int quantidade = nomes.Count;
nomes.Clear();
```

***

### **Dicionários (`Dictionary<TKey, TValue>`)**

Armazena **pares chave/valor**.

```csharp
using System.Collections.Generic;

Dictionary<string, int> idades = new Dictionary<string, int>();
idades["Lucas"] = 30;
idades["Ana"] = 25;

Console.WriteLine(idades["Lucas"]); // 30

foreach (var par in idades)
{
    Console.WriteLine($"Nome: {par.Key}, Idade: {par.Value}");
}
```

#### Métodos úteis:

```csharp
csharpCopiarEditaridades.Remove("Ana");
bool existe = idades.ContainsKey("Lucas");
```

***

### **Outras Coleções Úteis**

#### Fila – `Queue<T>` (FIFO)

```csharp
using System.Collections.Generic;

Queue<string> fila = new Queue<string>();
fila.Enqueue("Primeiro");
fila.Enqueue("Segundo");

Console.WriteLine(fila.Dequeue()); // Primeiro
```

> Primeiro a entrar é o primeiro a sair.

***

#### Pilha – `Stack<T>` (LIFO)

```csharp
Stack<string> pilha = new Stack<string>();
pilha.Push("Topo 1");
pilha.Push("Topo 2");

Console.WriteLine(pilha.Pop()); // Topo 2
```

> Último a entrar é o primeiro a sair.

***

#### Conjunto – `HashSet<T>` (sem duplicatas)

```csharp
HashSet<string> conjunto = new HashSet<string>();
conjunto.Add("A");
conjunto.Add("B");
conjunto.Add("A"); // Ignorado (duplicado)

foreach (var item in conjunto)
{
    Console.WriteLine(item); // A, B
}
```

> Útil para verificar **existência rápida** sem elementos repetidos.

***

#### Lista ordenada – `SortedList<TKey, TValue>`

```csharp
SortedList<int, string> ordenada = new SortedList<int, string>();
ordenada.Add(2, "B");
ordenada.Add(1, "A");
ordenada.Add(3, "C");

foreach (var item in ordenada)
{
    Console.WriteLine($"{item.Key} => {item.Value}");
}
// 1 => A
// 2 => B
// 3 => C
```

## Veja tambem

- [Big O e Complexidade](../../../computer-science/algoritmos/big-o-e-complexidade.md)
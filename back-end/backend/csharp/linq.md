---
description: Language Integrated Query - Query de Linguagem Integrada
---

# LinQ

## Introdução

LINQ é uma tecnologia introduzida na linguagem C# que permite a criação de consultas diretamente no código. Essa ferramenta unifica a forma de consultar dados de diversas fontes utilizando uma sintaxe mais familiar por ser escrita diretamente com código C#.

Podemos utilizar o LINQ para:

* Consultas em coleções: filtrar, ordenar, agrupar e projetar dados em listas, arrays e outras estruturas de dados;
* Acesso a bancos de dados: interagir com bancos de dados relacionais utilizando uma sintaxe semelhante ao SQL, mas com a tipagem forte e segurança do C#;
* Manipulação de XML: consultar e transformar dados XML de forma eficiente;
* Integração com outras fontes de dados: acessar dados de serviços web, objetos dinâmicos e outras fontes utilizando provedores LINQ personalizados.

## Operadores de consulta padrão

Os operadores de consulta padrão são as palavras-chave e métodos que formam o padrão LINQ. A maioria dos métodos opera em sequências, onde uma sequência representa um objeto cujo tipo implementa a interface `IEnumerable<T>` ou a interface `IQueryable<T>`.

Esses métodos fornecem recursos de consulta, incluindo filtros, projeção, agregação, classificação, entre outros. E eles são definidos como métodos de extensão do tipo que operam.

* `from`: especifica uma fonte de dados e uma variável de intervalo;
* `where`: filtra elementos de origem baseados em uma ou mais expressões booleanas;
* `select`: especifica o tipo e a forma que os elementos na sequência retornada terão;
* `group`: agrupa os resultados da consulta de acordo cum um valor de chave especificado.

## Sintaxe de Consulta x Sintaxe de Método

O LINQ oferece duas sintaxes principais para realizar consultas em coleções: a **sintaxe de método** e a **sintaxe de consulta**. Ambas as sintaxes vão produzir os mesmos resultados, mas possuem características e estilos diferentes.

> A **sintaxe de consulta** traz uma escrita mais próxima à linguagem SQL, utilizando palavras-chave como `from`, `where`, `select` e `orderby`, e aplica uma estrutura mais declarativa para especificar a consulta.

{% code title="Sintaxe de consulta" lineNumbers="true" %}
```csharp
var produtosCaros = from p in produtos
                   where p.Preco > 100
                   orderby p.Nome
                   select p;
```
{% endcode %}

> Já a **sintaxe de método** possui uma escrita mais semelhante à chamada de métodos tradicionais no C#, utilizando uma cadeia de chamadas de métodos para especificar a consulta, como no exemplo a seguir:

{% code title="Sintaxe do método" lineNumbers="true" %}
```csharp
var produtosCaros = produtos
    .Where(p => p.Preco > 100)
    .OrderBy(p => p.Nome)
    .ToList();
```
{% endcode %}

Os dois exemplos anteriores trarão exatamente o mesmo resultado

## Operadores de conversão

Os operadores de conversão no LINQ permitem transformar elementos de uma sequência em outro tipo de dados durante uma consulta.

As operações de conversão são úteis em diversos aspectos, como as aplicações dos métodos abaixo:

* `Enumerable.AsEnumerable`: oculta a implementação personalizada de um tipo de um operador de consulta padrão;
* `Enumerable.ToList`: força a execução de consulta imediata;
* `Enumerable.ToDictionary`: força a execução de consulta imediata.

Os métodos que começam com `As` alteram o tipo estático da coleção de origem, mas não a enumeram. Já os métodos cujos nomes começam com `To` enumeram a coleção de origem e colocam os itens na coleção de tipo correspondente.

## Projeções

Quando você usa LINQ para consultar coleções, você pode projetar os dados para uma forma diferente, como selecionar apenas algumas propriedades de um objeto ou até mesmo criar novos tipos de objetos com base nos dados da consulta.

Um exemplo simples de projeção seria pegar uma lista de objetos e projetar apenas algumas das suas propriedades:

#### **Projeção Normal:**

Imagine que você tem uma lista de objetos do tipo `Pessoa` com propriedades `Nome` e `Idade` e deseja selecionar apenas o nome de cada pessoa.

{% code title="Projeção Normal" lineNumbers="true" %}
```csharp
var pessoas = new List<Pessoa>
{
    new Pessoa { Nome = "Ana", Idade = 30 },
    new Pessoa { Nome = "Carlos", Idade = 25 },
    new Pessoa { Nome = "João", Idade = 40 }
};

var nomes = from p in pessoas
            select p.Nome;

foreach (var nome in nomes)
{
    Console.WriteLine(nome);
}
```
{% endcode %}

Neste exemplo, a projeção é feita através do `select p.Nome`, onde estamos projetando a lista de `Pessoa` para apenas os nomes.

#### Projeção Anônima:

A **projeção anônima** é um tipo especial de projeção em que você cria um novo tipo sem precisar declarar explicitamente uma classe para ele. Isso é feito através de tipos anônimos em C#, que permitem criar objetos temporários com propriedades que são definidos dinamicamente na consulta LINQ.

No caso de uma projeção anônima, você cria um novo objeto sem a necessidade de uma classe pré-existente. Um tipo anônimo é representado por `new {}`. Isso é útil quando você só precisa de um tipo específico para a consulta e não deseja criar uma classe separada.

Usando o mesmo exemplo de lista de `Pessoa`, vamos projetar um tipo anônimo que retorna tanto o nome quanto a idade de cada pessoa:

{% code title="Projeção Anônima" lineNumbers="true" %}
```csharp
var pessoas = new List<Pessoa>
{
    new Pessoa { Nome = "Ana", Idade = 30 },
    new Pessoa { Nome = "Carlos", Idade = 25 },
    new Pessoa { Nome = "João", Idade = 40 }
};

var resultado = from p in pessoas
                select new { p.Nome, p.Idade };

foreach (var item in resultado)
{
    Console.WriteLine($"Nome: {item.Nome}, Idade: {item.Idade}");
}
```
{% endcode %}

Neste exemplo, a projeção anônima é feita com `new { p.Nome, p.Idade }`, criando um objeto anônimo que possui as propriedades `Nome` e `Idade` para cada pessoa. Isso é útil quando você precisa de um tipo temporário apenas para essa consulta e não quer ou não precisa criar uma nova classe para isso.

## Transformação de Dados usando SelectMany( )

O método `SelectMany` no LINQ é útil quando estamos lidando com coleções aninhadas. Diferente do `Select`, que simplesmente projeta cada elemento de uma coleção para outra forma, o `SelectMany` realiza essa projeção e ao mesmo tempo achata a estrutura da coleção.

Imagine que temos uma lista de listas ou uma coleção onde cada elemento contém uma subcoleção (tipo lista dentro de lista ou objetos que possuem coleções). O `SelectMany` nos permite extrair os elementos dessas subcoleções e apresentá-los como uma única sequência contínua, simplificando o acesso aos dados.

Ele aplica uma projeção a cada elemento de uma sequência e, em seguida, concatena os resultados em uma única coleção.

> Exemplo 1

{% code title="Listas aninhadas" lineNumbers="true" %}
```csharp
var listaDeListas = new List<List<int>>
{
    new List<int> { 1, 2, 3 },
    new List<int> { 4, 5, 6 },
    new List<int> { 7, 8, 9 }
};

// Sem SelectMany ? Mantém estrutura de listas dentro de listas
var semAchatar = listaDeListas
                 .Select(lista => lista);
Console.WriteLine(string.Join(", ", semAchatar)); // Não imprime os números diretamente

// Com SelectMany ? "Achatando" a estrutura
var numerosAchatados = listaDeListas
                     .SelectMany(lista => lista);
Console.WriteLine(string.Join(", ", numerAchatados)); // Saída: 1, 2, 3, 4, 5, 6, 7, 8, 9
```
{% endcode %}

> Exemplo 2

Se tivermos uma lista de objetos, cada um contendo uma coleção interna, podemos usar `SelectMany` para extrair informações específicas.

{% code title="Transformação de dados" lineNumbers="true" %}
```csharp
class Aluno
{
    public string Nome { get; set; }
    public List<int> Notas { get; set; }
}

var alunos = new List<Aluno>
{
    new Aluno { Nome = "Ana", Notas = new List<int> { 8, 9, 10 } },
    new Aluno { Nome = "Carlos", Notas = new List<int> { 6, 7, 8 } },
    new Aluno { Nome = "João", Notas = new List<int> { 9, 9, 10 } }
};

// Extraindo todas as notas em uma única sequência
var todasAsNotas = alunos
                   .SelectMany(a => a.Notas);

Console.WriteLine(string.Join(", ", todasAsNotas)); // Saída: 8, 9, 10, 6, 7, 8, 9, 9, 10
```
{% endcode %}

> Exemplo 3

Transformar dados enquanto achatamos a estrutura.

{% code title="Projeção personalizada" lineNumbers="true" %}
```csharp
var resultado = alunos
                .SelectMany(a => a.Notas, (aluno, nota) 
                => new 
                { 
                aluno.Nome, Nota = nota 
                });

foreach (var item in resultado)
{
    Console.WriteLine($"{item.Nome}: {item.Nota}");
}
```
{% endcode %}

{% code title="Saída no terminal" %}
```textile
Ana: 8
Ana: 10
Carlos: 6
Carlos: 8
João: 9
João: 10
```
{% endcode %}

## Operações em conjunto

As operações em conjunto permitem que a gente trabalhe com uma ou mais fonte de dados e que a gente retorne um conjunto com resultados específicos que nós estamos buscando de acordo com a nossa necessidade.

## Veja tambem

- [Big O e Complexidade](../../../computer-science/algoritmos/big-o-e-complexidade.md)

Nas operações em conjunto a gente trabalha com quatro operadores.

* `Distinct()`: retorna uma sequência que contém elementos distintos da sequência original, com base em uma chave de comparação especificada;
* `Except()`: retorna uma sequência que contém elementos da primeira sequência que não estão na segunda sequência, com base em uma chave de comparação especificada;
* `Intersect()`: retorna uma sequência que contém elementos que estão presentes em ambas as sequências, com base em uma chave de comparação especificada;
* `Union()`: retorna uma sequência que contém elementos distintos das duas sequências, com base em uma chave de comparação especificada.

> E cada um destes operadores são usados em diferentes situações:

* `Distinct()`: quando você deseja remover elementos duplicados de uma sequência, com base em uma chave de comparação;
* `Except()`: quando você deseja encontrar elementos que estão presentes em uma sequência, mas não em outra, com base em uma chave de comparação;
* `Intersect()`: quando você deseja encontrar elementos que estão presentes em ambas as sequências, com base em uma chave de comparação;
* `Union()`: quando você deseja combinar elementos de duas sequências, removendo duplicados com base em uma chave de comparação.

## Operadores de ordenação

Os operadores de ordenação no LINQ permitem que você ordene elementos de uma sequência em ordem crescente ou decrescente, com base em uma ou mais propriedades ou expressões.

Os principais operadores de ordenação são:

* `OrderBy`: ordena os elementos da sequência em ordem crescente, com base na chave especificada pela função `keySelector`;
* `OrderByDescending`: ordena os elementos da sequência em ordem decrescente, com base na chave especificada pela função `keySelector`;
* `ThenBy`: adiciona um nível de ordenação secundária à sequência já ordenada;
* `ThenByDescending`: adiciona um nível de ordenação secundária decrescente à sequência já ordenada.

Estes operadores são utilizados para operação específicas, como:

* `OrderBy`: para ordenar os elementos em ordem crescente;
* `OrderByDescending`: para ordenar os elementos em ordem decrescente;
* `ThenBy`, `ThenByDescending`: para adicionar níveis de ordenação secundária

## Operações de agregação e agrupamento

Os métodos de agregação e agrupamento no LINQ permitem que sejam realizadas operações complexas sobre coleções de dados, permitindo que você resuma, combine e categorize informações de maneira eficiente.

Os métodos de agregação calculam um único valor a partir de uma sequência de valores. Alguns dos métodos de agregação mais comuns são:

* `Count()`: conta o número de elementos em uma sequência;
* `Sum()`: calcula a soma de todos os elementos numéricos em uma sequência;
* `Average()`: calcula a média aritmética de todos os elementos numéricos em uma sequência;
* `Min()`: retorna o menor valor em uma sequência;
* `Max()`: retorna o maior valor em uma sequência;
* `Aggregate()`: permite realizar cálculos personalizados sobre uma sequência, aplicando uma função de agregação cumulativa.

Já os métodos de agrupamento dividem uma sequência em grupos com base em uma chave comum. O método principal para agrupamento é o `GroupBy()` que agrupa os elementos de uma sequência com base na chave especificada pela função `keySelector`.

Você pode combinar métodos de agregação e agrupamento para realizar cálculos mais complexos em cada grupo. Por exemplo, para calcular a idade média de cada grupo de pessoas por idade:

```csharp
var idadeMediaPorGrupo = pessoasPorIdade
    .Select(grupo => new
    {
        Idade = grupo.Key,
        IdadeMedia = grupo.Average(p => p.Idade)
    });
```

### Trabalhando com `IEnumerable<T>` vs `IQueryable<T>`

| Interface        | Onde é usada                              | Execução                      | Exemplo                        |
| ---------------- | ----------------------------------------- | ----------------------------- | ------------------------------ |
| `IEnumerable<T>` | Coleções em memória (listas, arrays)      | **Imediata**                  | `List<T>`, `Array`             |
| `IQueryable<T>`  | Consultas a banco de dados (ORMs como EF) | **Adiada, convertida em SQL** | `DbSet<T>` no Entity Framework |

#### Exemplo com `IEnumerable<T>`

```csharp
List<string> nomes = new List<string> { "Ana", "Bruno", "Carlos" };
var resultado = nomes.Where(n => n.StartsWith("A")); // LINQ sobre lista
```

#### Exemplo com `IQueryable<T>`

```csharp
csharpCopiarEditar// Suponha um contexto Entity Framework
IQueryable<Usuario> usuarios = dbContext.Usuarios;
var ativos = usuarios.Where(u => u.Ativo); // LINQ vira SQL no banco
```

***

### ?? Outras operações úteis

| Método                         | Descrição                   |
| ------------------------------ | --------------------------- |
| `First()` / `FirstOrDefault()` | Retorna o primeiro elemento |
| `Any()` / `All()`              | Verifica condição booleana  |
| `Count()`                      | Conta elementos             |
| `Distinct()`                   | Remove duplicatas           |
| `Take(n)` / `Skip(n)`          | Pega/pula n elementos       |
| `ToList()` / `ToArray()`       | Converte o resultado        |

```csharp
var primeiros = nomes.Take(2).ToList();
bool existeLucas = nomes.Any(n => n == "Lucas");
```
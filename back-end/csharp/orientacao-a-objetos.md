# Orientação a Objetos

```csharp
public class Pessoa
{
    public string Nome;
    public int Idade;

    public void Falar()
    {
        Console.WriteLine($"Olá, meu nome é {Nome}.");
    }
}
```

Objeto: instância de uma classe

```csharp
Pessoa p1 = new Pessoa();
p1.Nome = "Lucas";
p1.Idade = 30;
p1.Falar(); // Olá, meu nome é Lucas.
```

***

### **Encapsulamento**

Controla o **acesso aos membros da classe** usando modificadores:

#### `private` – visível só dentro da classe

`protected` – visível na classe e herdeiros

`public` – visível de qualquer lugar

`internal` – visível só dentro do mesmo projeto/assembly

```csharp
public class ContaBancaria
{
    private double saldo = 1000.0;

    public void Depositar(double valor)
    {
        saldo += valor;
    }

    public double ConsultarSaldo()
    {
        return saldo;
    }
}
```

> Aqui, `saldo` é protegido de acesso direto (encapsulamento), mas acessível indiretamente via métodos públicos.

***

### **Herança**

Permite que uma classe herde atributos e métodos de outra.

```csharp
public class Animal
{
    public void Comer()
    {
        Console.WriteLine("Animal comendo...");
    }
}

public class Cachorro : Animal
{
    public void Latir()
    {
        Console.WriteLine("Au Au!");
    }
}
```

```csharp
Cachorro c = new Cachorro();
c.Comer(); // método herdado
c.Latir();
```

***

### **Polimorfismo**

#### **Sobrecarga (overload)** – mesmo nome, parâmetros diferentes

```csharp
public class Calculadora
{
    public int Somar(int a, int b) => a + b;
    public double Somar(double a, double b) => a + b;
}
```

#### **Sobrescrita (override)** – herdeiro reimplementa método da base

```csharp
public class Animal
{
    public virtual void FazerSom()
    {
        Console.WriteLine("Som genérico");
    }
}

public class Gato : Animal
{
    public override void FazerSom()
    {
        Console.WriteLine("Miau");
    }
}
```

***

### **Abstração**

Foca apenas nos aspectos essenciais de um objeto.

#### Classe abstrata (não pode ser instanciada)

```csharp
public abstract class Veiculo
{
    public abstract void Mover();
}
```

#### Implementação em subclasse

```csharp
public class Carro : Veiculo
{
    public override void Mover()
    {
        Console.WriteLine("O carro está se movendo");
    }
}
```

#### Interface – contrato de métodos

```csharp
public interface IImprimivel
{
    void Imprimir();
}

public class Documento : IImprimivel
{
    public void Imprimir()
    {
        Console.WriteLine("Documento impresso.");
    }
}
```

> Uma classe pode implementar várias interfaces, mas só herdar de uma classe.

***

### **Construtores e Destrutores**

#### Construtor – executado ao criar o objeto

```csharp
public class Produto
{
    public string Nome;
    public Produto(string nome)
    {
        Nome = nome;
    }
}
```

```csharp
Produto p = new Produto("Caneta");
```

#### Destrutor – raramente usado

```csharp
~Produto()
{
    // Liberar recursos (não determinístico)
}
```

> Use com moderação; o **garbage collector** já cuida da maioria dos casos.

***

### **this** e **base**

#### `this` → referencia a si mesmo

```csharp
public class Pessoa
{
    private string nome;

    public Pessoa(string nome)
    {
        this.nome = nome; // desambiguando o campo do parâmetro
    }
}
```

#### `base` → referencia à classe base

```csharp
public class Animal
{
    public Animal()
    {
        Console.WriteLine("Animal criado");
    }
}

public class Gato : Animal
{
    public Gato() : base() // chama o construtor da base
    {
        Console.WriteLine("Gato criado");
    }
}
```

***

### Modificadores Especiais

| Modificador | Uso                                                  |
| ----------- | ---------------------------------------------------- |
| `sealed`    | Impede que uma classe seja herdada                   |
| `static`    | A classe ou membro pertence ao tipo, não à instância |
| `virtual`   | Permite sobrescrever o método na herança             |
| `override`  | Substitui o método `virtual` da base                 |
| `new`       | Oculta um membro da classe base (não sobrescreve)    |



```csharp
public sealed class Sistema
{
    // não pode ser herdada
}

public class Matematica
{
    public static double Pi = 3.14;
    public static double Quadrado(double x) => x * x;
}
```

```csharp
public class Animal
{
    public virtual void Som() => Console.WriteLine("Som do animal");
}

public class Leao : Animal
{
    public override void Som() => Console.WriteLine("Rugido");
}
```

```csharp
public class Base
{
    public void Mostrar() => Console.WriteLine("Base");
}

public class Derivada : Base
{
    public new void Mostrar() => Console.WriteLine("Derivada"); // oculta, não sobrescreve
}
```

####

#### **LINQ (Language Integrated Query)**

* Sintaxe de consulta vs. sintaxe de método
* Operações comuns: `Select`, `Where`, `OrderBy`, `GroupBy`, `Aggregate`
* Operações com `IEnumerable<T>` e `IQueryable<T>`

#### **Tratamento de Erros**

* `try`, `catch`, `finally`, `throw`
* Tipos de exceções (`Exception`, `ArgumentException`, `NullReferenceException`...)

#### **Eventos e Delegados**

* Delegados (`delegate`)
* Eventos (`event`)
* Action, Func, Predicate

#### **Programação Assíncrona**

* `async` e `await`
* `Task`, `ValueTask`
* Cancelamento (`CancellationToken`)
* `ConfigureAwait(false)`

#### **Manipulação de Arquivos**

* `System.IO`: `File`, `Directory`, `FileStream`, `StreamReader`, `StreamWriter`

**Expressões Lambda**

* Sintaxe e uso
* Uso com LINQ e eventos

#### &#x20;**Tópicos Avançados**

* Reflection
* Generics (`List<T>`, `T`, `where T : class`)
* Nullable types (`int?`, `Nullable<T>`)
* Tuplas (`(int, string)`, `ValueTuple`)
* Records (C# 9+)
* Pattern Matching
* Extension methods
* Attributes e Annotations

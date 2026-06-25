---
description: >-
  C# é uma linguagem orientada a objetos da Microsoft, moderna, fortemente
  tipada, usada principalmente para desenvolvimento na plataforma .NET.
---

# C\#

```csharp
using System;

namespace Tranquilidade
{
    class Programa
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Eaí pessoal, tudo tranquilo?");

            int numero = 10;
            double numeroDecimal = 20.5;
            string texto = "C# é poderoso!";
            bool csharpEhDivertido = true;

            Console.WriteLine($"Inteiro: {numero}");
            Console.WriteLine($"Decimal: {numeroDecimal}");
            Console.WriteLine($"Texto: {texto}");
            Console.WriteLine($"Booleano: {csharpEhDivertido}");

            for (int i = 0; i < 5; i++)
            {
                Console.WriteLine($"Esta é a iteração número {i} do loop");
            }

            int resultadoSoma = Somar(5, 10);
            Console.WriteLine($"A soma de 5 e 10 é: {resultadoSoma}");
        }

        static int Somar(int a, int b)
        {
            return a + b;
        }
    }
}
```
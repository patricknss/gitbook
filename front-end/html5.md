---
description: >-
  HTML5 é a quinta versão da linguagem de marcação HTML (HyperText Markup
  Language), usada para estruturar e apresentar conteúdo na web.
---

# HTML5

## **Estrutura Básica**

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Título da Página</title>
  <link rel="stylesheet" href="estilo.css"> <!-- Opcional -->
</head>
<body>
  
  <header>
    <h1>Bem-vindo ao meu site</h1>
  </header>

  <nav>
    <ul>
      <li><a href="#">Início</a></li>
      <li><a href="#">Sobre</a></li>
      <li><a href="#">Contato</a></li>
    </ul>
  </nav>

  <main>
    <section>
      <h2>Seção principal</h2>
      <p>Conteúdo principal da página.</p>
    </section>
  </main>

  <footer>
    <p>&copy; Trick.N - 2025 - Todos os direitos reservados</p>
  </footer>

</body>
</html>
```

| `<!DOCTYPE html>` | Define que o documento usa HTML5 |
| ----------------- | -------------------------------- |

| `<html>` | Elemento raiz do documento HTML |
| -------- | ------------------------------- |

| `<head>` | Contém metadados (título, charset, links, etc.) |
| -------- | ----------------------------------------------- |

| `<meta charset>` | Define a codificação (UTF-8 = suporta acentuação) |
| ---------------- | ------------------------------------------------- |

| `<meta viewport>` | Garante responsividade em dispositivos móveis |
| ----------------- | --------------------------------------------- |

| `<title>` | Título exibido na aba do navegador |
| --------- | ---------------------------------- |

| `<body>` | Onde fica o conteúdo visível da página |
| -------- | -------------------------------------- |

| `<header>` | Cabeçalho da página ou seção |
| ---------- | ---------------------------- |

| `<nav>` | Menu de navegação |
| ------- | ----------------- |

| `<main>` | Conteúdo principal (somente um por página) |
| -------- | ------------------------------------------ |

| `<section>` | Seções temáticas |
| ----------- | ---------------- |

| `<footer>` | Rodapé com informações finais |
| ---------- | ----------------------------- |

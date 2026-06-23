---
description: >-
  CSS é uma linguagem de estilo usada para definir a aparência visual de páginas
  HTML. Permite separar conteúdo (HTML) da apresentação (cores, fontes, layout).
---

# CSS

## Estrutura Básica

```css
/* Comentários são escritos assim */

/* Seletor de elemento */
body {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  color: #333;
}

/* Seletor de ID */
#cabecalho {
  background-color: #007bff;
  color: white;
  padding: 20px;
}

/* Seletor de classe */
.caixa {
  background-color: white;
  border: 1px solid #ccc;
  padding: 15px;
  margin: 10px;
  border-radius: 8px;
}

/* Seletor múltiplo */
h1, h2, h3 {
  font-weight: normal;
}

/* Pseudo-classe */
a:hover {
  color: red;
  text-decoration: underline;
}

/* Layout com Flexbox (exemplo básico) */
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

## **Três formas de usar o CSS com HTML**

> **Interno (dentro do HTML):**

```html
<style>
  body {
    background-color: #eee;
  }
</style>
```

> **Externo (arquivo separado):**

```html
<link rel="stylesheet" href="estilo.css">
```

> **Inline (no próprio elemento):**

```html
<p style="color: blue;">Texto azul</p>
```

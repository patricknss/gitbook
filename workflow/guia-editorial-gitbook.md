# Guia Editorial do GitBook

Padrao para manter os conteudos consistentes, legiveis e navegaveis.

## Estrutura minima de cada pagina

1. Titulo objetivo.
2. Contexto curto (2-4 linhas).
3. Topicos principais.
4. Exemplo pratico (quando fizer sentido).
5. Erros comuns/checklist.
6. Referencias cruzadas.

## Padrao de escrita

- Linguagem direta, sem excesso de teoria.
- Frases curtas e foco no "como fazer".
- Termos tecnicos com nome consistente em todo o repositório.
- Evitar jargao sem explicacao.

## Padrao de formatacao

- Usar `##` e `###` para hierarquia.
- Usar listas numeradas para fluxo.
- Usar bloco de codigo com linguagem (`csharp`, `sql`, `bash`, `xml`).
- Evitar HTML inline para cor como regra geral.

## Cores e destaque

- Priorizar Markdown puro (`**negrito**`, blocos de citação, tabelas).
- Evitar hardcode de cor com `<mark style=...>` em novos textos.
- Se precisar destaque visual, usar no maximo 1 estrategia por secao.

## Links cruzados

- Toda pagina nova deve apontar para ao menos 2 paginas relacionadas.
- Preferir links relativos internos.
- Nome do link deve ser descritivo (nao usar "clique aqui").

## Checklist antes de publicar

1. O texto responde "o que e", "quando usar" e "como aplicar"?
2. Os exemplos funcionam no contexto descrito?
3. Os links internos existem?
4. O conteudo evita contradizer guias existentes?
5. Ha secao de risco/erro comum para operacoes sensiveis?

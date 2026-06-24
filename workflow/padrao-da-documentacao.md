# Padrão de manutenção da documentação

Este repositório é uma base de **anotações do que foi aprendido**.  
Para manter qualidade e consistência, seguir este padrão em toda atualização.

## Estrutura mínima por página

1. Contexto curto do tema.
2. Quando usar / por que importa.
3. Exemplo prático (código, comando ou fluxo).
4. Boas práticas / cuidados.
5. Versão de referência (quando aplicável).

## Regra obrigatória de atualização

Sempre que qualquer página for criada ou alterada:

1. Atualizar o `SUMMARY.md` (navegação).
2. Atualizar `useful-links.md` com referências relevantes.
3. Garantir que exemplos estejam alinhados com versões atuais.

## Checklist obrigatório no PR

Toda PR de documentação deve confirmar:

- [ ] `SUMMARY.md` atualizado (navegação ok).
- [ ] `useful-links.md` atualizado com novas referências.
- [ ] Página segue estrutura mínima (contexto, uso, exemplo, boas práticas, versão/referências).
- [ ] Exemplos revisados e coerentes com o tema.

## Convenções deste projeto

- Linguagem: português claro e direto.
- Foco: documentação prática e reutilizável.
- Não usar conteúdo como "trilha de estudos futura"; descrever o que já foi estudado/aplicado.

## Checklist rápido antes de publicar

- Link da página presente no `SUMMARY.md`.
- Exemplo executável ou reproduzível.
- Referências externas adicionadas em `useful-links.md`.
- Título e conteúdo coerentes com a proposta de anotações.

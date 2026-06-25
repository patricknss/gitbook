---
description: >-
  Nesta página, vamos embarcar em uma jornada abrangente pelo mundo do SQL
  (Structured Query Language) e explorar os princípios fundamentais da
  linguagem.
icon: database
---

# Banco de Dados

## Contexto

Notas e exemplos sobre SQL e versionamento de schema com foco prático.

## Versões de referência (jun/2026)

- SQL: conceitos ANSI e compatíveis com bancos relacionais modernos.
- Liquibase Community: **5.0.3**.

## O que está documentado

1. Conceitos fundamentais de SQL (SELECT, INSERT, UPDATE, DELETE).
2. Joins básicos e filtros.
3. Modelagem simples de dados.
4. Introdução ao uso de Liquibase para versionamento de schema.

## Exemplo prático

Cenário: sistema de loja.

1. Modelar tabelas `clientes`, `produtos`, `pedidos`.
2. Inserir dados com `INSERT`.
3. Consultar pedidos por cliente com `SELECT` + `JOIN`.
4. Corrigir preço com `UPDATE`.
5. Versionar alterações de schema com Liquibase.

## Referências

- [Useful Links](../../useful-links.md)
- [Backend (Fundamentos e APIs)](../../back-end/backend/README.md)
- [Bibliotecas](../../bibliotecas/README.md)
- [Dapper](../../bibliotecas/dapper.md)
- [NHibernate](../../bibliotecas/nhibernate.md)

---

# SQL

## O que é SQL e para que serve? <a href="#o-que-e-sql-e-para-que-serve" id="o-que-e-sql-e-para-que-serve"></a>

**O SQL é uma linguagem padrão para trabalhar com bancos de dados relacionais, amplamente utilizada por profissionais em diversas áreas, desde cientistas de dados até pessoas usuárias de Excel.**

E, ao contrário do que muitas pessoas pensam, para escrever queries (ou, como chamam, consultas) e ter resultados positivos, não precisa conhecer profundamente sobre programação.

SQL é a sigla para _<mark style="color:#C62828;">Structured Query Language</mark>_.

A linguagem SQL é relativamente semelhante entre os principais Sistemas Gerenciadores de Banco de Dados (<mark style="color:#B26A00;">SGBDs</mark>) do mercado, como: <mark style="color:#2E7D32;">Oracle, MySQL, MariaDB, PostgreSQL, Microsoft SQL Server,</mark> entre outros.

No entanto, é muito importante destacar que cada um deles têm suas características e particularidades distintas. Por exemplo, o <mark style="color:#2E7D32;">MySQL</mark> e o <mark style="color:#2E7D32;">PostgreSQL</mark> são extremamente notáveis por oferecerem versões gratuitas e de código aberto, tornando-se extremamente populares por essa razão.

Um grande destaque do SQL é ser uma linguagem fundamental em várias profissões, especialmente para pessoas que querem se sobressair no mercado.

É o caso, por exemplo, de quem já usa o <mark style="color:#2E7D32;">Excel</mark> de maneira intensiva e precisa migrar suas informações para um banco de dados. Ou de cientistas de dados que usam Python para agregar dados em diferentes fontes de informações.

## Curiosidades do SQL <a href="#curiosidades-do-sql" id="curiosidades-do-sql"></a>

A evolução da <mark style="color:#1565C0;">linguagem SQL</mark> proporcionou desenvolvimentos significativos na gestão de dados e no processamento de informações.

Antes de todo esse conhecimento estar ao nosso alcance, a gestão de dados era um terreno extremamente desafiador, com sistemas de banco de dados específicos para cada aplicação.

## Antes do SQL

Nas décadas de 1960 e 1970, o contexto da gestão de dados era desafiador e fragmentado, pois cada aplicação tinha seu sistema de gerenciamento de banco de dados (<mark style="color:#C65D00;">SGBD</mark>) específico. E, geralmente, envolviam a programação direta de acesso a dados.

Nisto, cada aplicativo tinha seu próprio formato de armazenamento de dados e não havia uma linguagem comum para consulta e manipulação de informações.

A solução veio, então, com o desenvolvimento do <mark style="color:#2E7D32;">SQL</mark>, como uma linguagem comum para consulta e manipulação de informações.

Por trás disso, se estabeleceu um padrão que unificou a forma como as pessoas acessam e tratam dados, impactando o processo da gestão de informações.

## O Surgimento da SQL

Em meados da década de 1970, como você já sabe, a principal motivação para criar o <mark style="color:#2E7D32;">SQL</mark> foi o problema de complexidade dos sistemas de gerenciamento de dados.

A solução surgiu quando pesquisadores da IBM desenvolveram o <mark style="color:#C65D00;">SEQUEL</mark> (<mark style="color:#C62828;">Structured English Query Language</mark>), que serviu como base para o <mark style="color:#2E7D32;">SQL</mark> que temos acesso nos dias de hoje.

Em 1979, o <mark style="color:#C65D00;">SEQUEL</mark> foi renomeado para <mark style="color:#2E7D32;">SQL</mark> e a primeira especificação da linguagem foi lançada em 1986, tornando-se um padrão.

Ou seja, a criação do <mark style="color:#2E7D32;">SQL</mark> partiu da ideia de uma linguagem declarativa para consultar dados, em que as pessoas usuárias especificam o que querem ao invés de como obtê-lo.

Por esse caminho, o <mark style="color:#2E7D32;">SQL</mark> evoluiu significativamente ao longo dos anos, incorporando recursos avançados e expandindo sua aplicação para além de bancos de dados relacionais tradicionais.

Atualmente, o <mark style="color:#2E7D32;">SQL</mark> é usado em uma ampla gama de aplicativos, desde análise de dados até sistemas de informações de saúde e gerenciamento de conteúdo da web.

## Por que aprender SQL? <a href="#por-que-aprender-sql" id="por-que-aprender-sql"></a>

Além de banco de dados, a grande área de tecnologia envolve muitos outros segmentos. O grande ponto é que esses segmentos são interligados entre si.

Ou seja, existe uma certa dependência entre eles. A linguagem <mark style="color:#2E7D32;">SQL</mark> é, portanto, o padrão para o gerenciamento de bancos de dados relacionais que conecta uma ampla variedade de aplicações.

Como Dev de softwares que frequentemente precisam integrar bancos de dados em suas aplicações, o conhecimento de <mark style="color:#2E7D32;">SQL</mark> é fundamental para criar aplicativos que interajam de maneira eficaz com bancos de dados.

Assim, como em outras áreas, se você busca uma carreira em tecnologia, análise de dados, ciência de dados ou desenvolvimento de software, o <mark style="color:#2E7D32;">SQL</mark> é uma habilidade essencial amplamente valorizada pelas empresas.

## Quais são as vantagens e desvantagens da linguagem SQL? <a href="#quais-sao-as-vantagens-e-desvantagens-da-linguagem-sql" id="quais-sao-as-vantagens-e-desvantagens-da-linguagem-sql"></a>

Como ocorre em qualquer outra linguagem ou tecnologia, o <mark style="color:#2E7D32;">SQL</mark> possui vantagens e desvantagens que dependem do contexto e das necessidades específicas.

Para aprofundar nossa compreensão sobre as diversas possibilidades, vamos examinar algumas das principais vantagens e desvantagens da linguagem <mark style="color:#2E7D32;">SQL</mark>.

### Vantagens

Dentre todas as vantagens do <mark style="color:#2E7D32;">SQL</mark>, as principais são:

* <mark style="color:#1565C0;">**Padronização**</mark>**:** trata-se de uma linguagem padronizada, o que significa que, independentemente do sistema de gerenciamento de banco de dados (<mark style="color:#C65D00;">SGBD</mark>) que você está usando (por exemplo, <mark style="color:#2E7D32;">MySQL, PostgreSQL, Oracle, SQL Server</mark>), a linguagem em si é a mesma. Isso facilita a portabilidade e a aprendizagem.
* <mark style="color:#1565C0;">**Facilidade de Consulta**</mark>**:** oferece uma maneira intuitiva de consultar e recuperar dados de bancos de dados. É uma linguagem declarativa que permite que você especifique o que deseja, em vez de como obtê-lo, tornando as consultas mais compreensíveis.
* <mark style="color:#1565C0;">**Integridade de Dados**</mark>**:** permite a definição de restrições de integridade, como chaves primárias e estrangeiras, para garantir que os dados sejam consistentes e confiáveis.

### Desvantagens

Mas tudo tem dois lados. E o SQL também tem alguns pontos não tão bons assim:

* <mark style="color:#1565C0;">**Complexidade**</mark>**:** as consultas SQL mais complexas podem se tornar difíceis de escrever e entender, principalmente para iniciantes, tornando-as mais suscetíveis a erros.
* <mark style="color:#1565C0;">**Desempenho Variável**</mark>: o desempenho das consultas <mark style="color:#2E7D32;">SQL</mark> pode variar dependendo da estrutura do banco de dados, da indexação e do volume de dados. Consultas mal otimizadas podem ser lentas.
* <mark style="color:#1565C0;">**Falta de Suporte para dados não estruturados**</mark>**:** o <mark style="color:#2E7D32;">SQL</mark> é otimizado para dados estruturados, o que significa que não é a melhor escolha para armazenar e consultar dados não estruturados, como documentos de texto, áudio ou vídeo.

## Onde o SQL é usado? <a href="#onde-o-sql-e-usado" id="onde-o-sql-e-usado"></a>

A linguagem <mark style="color:#2E7D32;">SQL</mark> e os bancos de dados relacionais podem ser utilizados em várias áreas da nossa vida.

* <mark style="color:#1565C0;">**Desenvolvimento de Aplicativos Web**</mark>**:** muitos aplicativos web usam bancos de dados para armazenar informações. O <mark style="color:#2E7D32;">SQL</mark> pode ser utilizado para criar consultas que recuperam, inserem, atualizam e excluem dados em tempo real.
* <mark style="color:#1565C0;">**Análise de Dados e Business Intelligence (BI)**</mark>**:** A execução de consultas em bancos de dados para análise de dados, geração de relatórios e painéis de controle é um processo no qual o <mark style="color:#2E7D32;">SQL</mark> desempenha um papel fundamental. Ferramentas de BI geralmente usam SQL para recuperar informações com eficácia.
* <mark style="color:#1565C0;">**Ciência de Dados e Mineração de Dados**</mark>**:** Em ciência de dados, o <mark style="color:#2E7D32;">SQL</mark> é usado para limpar, transformar e analisar dados armazenados em bancos de dados. Ele é fundamental para consultas complexas e agregação de dados.
* <mark style="color:#1565C0;">**Aplicações Móveis**</mark>**:** Muitos aplicativos móveis usam bancos de dados embutidos que são acessados por meio de <mark style="color:#2E7D32;">SQL</mark> para armazenar dados localmente no dispositivo.
* <mark style="color:#1565C0;">**Educação e Pesquisa**</mark>**:** Instituições acadêmicas e de pesquisa usam <mark style="color:#2E7D32;">SQL</mark> para armazenar e analisar dados em uma ampla variedade de disciplinas, desde economia até biologia, contribuindo para avanços significativos no conhecimento através da resolução de problemas complexos, como a compreensão de tendências educacionais e o aprimoramento de métodos de ensino.
* <mark style="color:#1565C0;">**Gestão de Finanças e Contabilidade**</mark>**:** Empresas financeiras e departamentos de contabilidade usam <mark style="color:#2E7D32;">SQL</mark> para gerenciar e analisar dados financeiros, incluindo transações, balanços e demonstrações financeiras.

Como você pode perceber, atualmente, os dados desempenham um papel fundamental na nossa sociedade. No fim das contas, todos nós somos pessoas que consomem e, ativamente, produzem dados.

Assim, em diferentes contextos — da economia, da ciência, da saúde e da educação, a coleta e a análise de informações são pontos fundamentais para embasar decisões criteriosas e promover avanços significativos.

## Como o SQL funciona? <a href="#como-o-sql-funciona" id="como-o-sql-funciona"></a>

A linguagem <mark style="color:#2E7D32;">SQL</mark> serve para interagir com os nossos dados que estão armazenados dentro de um banco de dados relacional, permitindo a criação, a recuperação, a atualização e a exclusão desses dados.

O <mark style="color:#2E7D32;">SQL</mark> funciona, portanto, por meio de vários componentes e processos que trabalham juntos para gerenciar bancos de dados relacionais, como:

* <mark style="color:#1565C0;">Analisador</mark>: analisa a instrução <mark style="color:#2E7D32;">SQL</mark>, verificando sua sintaxe e convertendo palavras-chave em símbolos especiais.
* <mark style="color:#1565C0;">Exatidão</mark>: garante que a instrução esteja em conformidade com as regras da <mark style="color:#2E7D32;">SQL</mark>, como o uso correto de pontos e vírgulas.
* <mark style="color:#1565C0;">Autorização</mark>: valida se o usuário tem permissão para executar a consulta e manipular os dados, com base em suas credenciais.
* <mark style="color:#1565C0;">Mecanismo Relacional</mark>: cria um plano eficiente para acessar os dados, otimizando a consulta e verificando consultas semelhantes.
* <mark style="color:#1565C0;">Mecanismo de Armazenamento</mark>: executa a instrução <mark style="color:#2E7D32;">SQL</mark>, lendo e gravando os dados nos arquivos de armazenamento físico.

## Quais são os componentes de um sistema SQL? <a href="#quais-sao-os-componentes-de-um-sistema-sql" id="quais-sao-os-componentes-de-um-sistema-sql"></a>

Um sistema <mark style="color:#2E7D32;">SQL</mark> (<mark style="color:#C62828;">Sistema de Gerenciamento de Banco de Dados Relacionais</mark>) é composto por vários componentes que trabalham juntos para permitir a <mark style="color:#1565C0;">**criação**</mark>, a <mark style="color:#1565C0;">**manutenção**</mark> e a <mark style="color:#1565C0;">**consulta**</mark> de bancos de dados relacionais:

* <mark style="color:#1565C0;">Tabelas</mark>: as tabelas são estruturas fundamentais em um banco de dados <mark style="color:#2E7D32;">SQL</mark>. Elas são usadas para armazenar os dados em formato tabular, com colunas representando atributos e linhas representando registros. As tabelas são projetadas com base no modelo relacional.
* <mark style="color:#1565C0;">Procedimentos Armazenados</mark>: os procedimentos armazenados são blocos de código <mark style="color:#2E7D32;">SQL</mark> que podem ser definidos e armazenados no banco de dados. Eles são usados para realizar tarefas específicas, como cálculos complexos, validações de dados e processamento personalizado. Os procedimentos armazenados podem ser chamados por aplicativos ou outros procedimentos armazenados.
* <mark style="color:#1565C0;">Instruções SQL</mark>: As instruções <mark style="color:#2E7D32;">SQL</mark>, como <mark style="color:#2E7D32;">SELECT</mark>, <mark style="color:#2E7D32;">INSERT</mark>, <mark style="color:#2E7D32;">UPDATE</mark> e <mark style="color:#2E7D32;">DELETE</mark>, são usadas para manipular os dados em tabelas. As instruções <mark style="color:#2E7D32;">SQL</mark> permitem que os usuários realizem consultas, insiram novos dados, atualizem registros existentes e excluam dados.

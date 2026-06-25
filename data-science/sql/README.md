---
description: >-
  Nesta página, vamos embarcar em uma jornada abrangente pelo mundo do SQL
  (Structured Query Language) e explorar os princípios fundamentais da
  linguagem.
---

# Banco de Dados

## Banco de Dados

### Contexto

Notas e exemplos sobre SQL e versionamento de schema com foco prático.

### Versões de referência (jun/2026)

* SQL: conceitos ANSI e compatíveis com bancos relacionais modernos.
* Liquibase Community: **5.0.3**.

### O que está documentado

1. Conceitos fundamentais de SQL (SELECT, INSERT, UPDATE, DELETE).
2. Joins básicos e filtros.
3. Modelagem simples de dados.
4. Introdução ao uso de Liquibase para versionamento de schema.
5. Playbook operacional de Liquibase para release e rollback.

### Exemplo prático

Cenário: sistema de loja.

1. Modelar tabelas `clientes`, `produtos`, `pedidos`.
2. Inserir dados com `INSERT`.
3. Consultar pedidos por cliente com `SELECT` + `JOIN`.
4. Corrigir preço com `UPDATE`.
5. Versionar alterações de schema com Liquibase.

### Referências

- [Useful Links](../../useful-links.md)
- [Backend (Fundamentos e APIs)](../../back-end/backend/README.md)
- [Bibliotecas](../../bibliotecas/README.md)
- [Dapper](../../bibliotecas/dapper.md)
- [NHibernate](../../bibliotecas/nhibernate.md)
- [Liquibase: introducao](liquibase-introducao.md)
- [Liquibase Operacional](liquibase-operacional.md)

***

## SQL

### O que é SQL e para que serve? <a href="#o-que-e-sql-e-para-que-serve" id="o-que-e-sql-e-para-que-serve"></a>

**O SQL é uma linguagem padrão para trabalhar com bancos de dados relacionais, amplamente utilizada por profissionais em diversas áreas, desde cientistas de dados até pessoas usuárias de Excel.**

E, ao contrário do que muitas pessoas pensam, para escrever queries (ou, como chamam, consultas) e ter resultados positivos, não precisa conhecer profundamente sobre programação.

SQL é a sigla para _Structured Query Language_.

A linguagem SQL é relativamente semelhante entre os principais Sistemas Gerenciadores de Banco de Dados (SGBDs) do mercado, como: Oracle, MySQL, MariaDB, PostgreSQL, Microsoft SQL Server, entre outros.

No entanto, é muito importante destacar que cada um deles têm suas características e particularidades distintas. Por exemplo, o MySQL e o PostgreSQL são extremamente notáveis por oferecerem versões gratuitas e de código aberto, tornando-se extremamente populares por essa razão.

Um grande destaque do SQL é ser uma linguagem fundamental em várias profissões, especialmente para pessoas que querem se sobressair no mercado.

É o caso, por exemplo, de quem já usa o Excel de maneira intensiva e precisa migrar suas informações para um banco de dados. Ou de cientistas de dados que usam Python para agregar dados em diferentes fontes de informações.

### Curiosidades do SQL <a href="#curiosidades-do-sql" id="curiosidades-do-sql"></a>

A evolução da linguagem SQL proporcionou desenvolvimentos significativos na gestão de dados e no processamento de informações.

Antes de todo esse conhecimento estar ao nosso alcance, a gestão de dados era um terreno extremamente desafiador, com sistemas de banco de dados específicos para cada aplicação.

### Antes do SQL

Nas décadas de 1960 e 1970, o contexto da gestão de dados era desafiador e fragmentado, pois cada aplicação tinha seu sistema de gerenciamento de banco de dados (SGBD) específico. E, geralmente, envolviam a programação direta de acesso a dados.

Nisto, cada aplicativo tinha seu próprio formato de armazenamento de dados e não havia uma linguagem comum para consulta e manipulação de informações.

A solução veio, então, com o desenvolvimento do SQL, como uma linguagem comum para consulta e manipulação de informações.

Por trás disso, se estabeleceu um padrão que unificou a forma como as pessoas acessam e tratam dados, impactando o processo da gestão de informações.

### O Surgimento da SQL

Em meados da década de 1970, como você já sabe, a principal motivação para criar o SQL foi o problema de complexidade dos sistemas de gerenciamento de dados.

A solução surgiu quando pesquisadores da IBM desenvolveram o SEQUEL (Structured English Query Language), que serviu como base para o SQL que temos acesso nos dias de hoje.

Em 1979, o SEQUEL foi renomeado para SQL e a primeira especificação da linguagem foi lançada em 1986, tornando-se um padrão.

Ou seja, a criação do SQL partiu da ideia de uma linguagem declarativa para consultar dados, em que as pessoas usuárias especificam o que querem ao invés de como obtê-lo.

Por esse caminho, o SQL evoluiu significativamente ao longo dos anos, incorporando recursos avançados e expandindo sua aplicação para além de bancos de dados relacionais tradicionais.

Atualmente, o SQL é usado em uma ampla gama de aplicativos, desde análise de dados até sistemas de informações de saúde e gerenciamento de conteúdo da web.

### Por que aprender SQL? <a href="#por-que-aprender-sql" id="por-que-aprender-sql"></a>

Além de banco de dados, a grande área de tecnologia envolve muitos outros segmentos. O grande ponto é que esses segmentos são interligados entre si.

Ou seja, existe uma certa dependência entre eles. A linguagem SQL é, portanto, o padrão para o gerenciamento de bancos de dados relacionais que conecta uma ampla variedade de aplicações.

Como Dev de softwares que frequentemente precisam integrar bancos de dados em suas aplicações, o conhecimento de SQL é fundamental para criar aplicativos que interajam de maneira eficaz com bancos de dados.

Assim, como em outras áreas, se você busca uma carreira em tecnologia, análise de dados, ciência de dados ou desenvolvimento de software, o SQL é uma habilidade essencial amplamente valorizada pelas empresas.

### Quais são as vantagens e desvantagens da linguagem SQL? <a href="#quais-sao-as-vantagens-e-desvantagens-da-linguagem-sql" id="quais-sao-as-vantagens-e-desvantagens-da-linguagem-sql"></a>

Como ocorre em qualquer outra linguagem ou tecnologia, o SQL possui vantagens e desvantagens que dependem do contexto e das necessidades específicas.

Para aprofundar nossa compreensão sobre as diversas possibilidades, vamos examinar algumas das principais vantagens e desvantagens da linguagem SQL.

#### Vantagens

Dentre todas as vantagens do SQL, as principais são:

* **Padronização****:** trata-se de uma linguagem padronizada, o que significa que, independentemente do sistema de gerenciamento de banco de dados (SGBD) que você está usando (por exemplo, MySQL, PostgreSQL, Oracle, SQL Server), a linguagem em si é a mesma. Isso facilita a portabilidade e a aprendizagem.
* **Facilidade de Consulta****:** oferece uma maneira intuitiva de consultar e recuperar dados de bancos de dados. É uma linguagem declarativa que permite que você especifique o que deseja, em vez de como obtê-lo, tornando as consultas mais compreensíveis.
* **Integridade de Dados****:** permite a definição de restrições de integridade, como chaves primárias e estrangeiras, para garantir que os dados sejam consistentes e confiáveis.

#### Desvantagens

Mas tudo tem dois lados. E o SQL também tem alguns pontos não tão bons assim:

* **Complexidade****:** as consultas SQL mais complexas podem se tornar difíceis de escrever e entender, principalmente para iniciantes, tornando-as mais suscetíveis a erros.
* **Desempenho Variável**: o desempenho das consultas SQL pode variar dependendo da estrutura do banco de dados, da indexação e do volume de dados. Consultas mal otimizadas podem ser lentas.
* **Falta de Suporte para dados não estruturados****:** o SQL é otimizado para dados estruturados, o que significa que não é a melhor escolha para armazenar e consultar dados não estruturados, como documentos de texto, áudio ou vídeo.

### Onde o SQL é usado? <a href="#onde-o-sql-e-usado" id="onde-o-sql-e-usado"></a>

A linguagem SQL e os bancos de dados relacionais podem ser utilizados em várias áreas da nossa vida.

* **Desenvolvimento de Aplicativos Web****:** muitos aplicativos web usam bancos de dados para armazenar informações. O SQL pode ser utilizado para criar consultas que recuperam, inserem, atualizam e excluem dados em tempo real.
* **Análise de Dados e Business Intelligence (BI)****:** A execução de consultas em bancos de dados para análise de dados, geração de relatórios e painéis de controle é um processo no qual o SQL desempenha um papel fundamental. Ferramentas de BI geralmente usam SQL para recuperar informações com eficácia.
* **Ciência de Dados e Mineração de Dados****:** Em ciência de dados, o SQL é usado para limpar, transformar e analisar dados armazenados em bancos de dados. Ele é fundamental para consultas complexas e agregação de dados.
* **Aplicações Móveis****:** Muitos aplicativos móveis usam bancos de dados embutidos que são acessados por meio de SQL para armazenar dados localmente no dispositivo.
* **Educação e Pesquisa****:** Instituições acadêmicas e de pesquisa usam SQL para armazenar e analisar dados em uma ampla variedade de disciplinas, desde economia até biologia, contribuindo para avanços significativos no conhecimento através da resolução de problemas complexos, como a compreensão de tendências educacionais e o aprimoramento de métodos de ensino.
* **Gestão de Finanças e Contabilidade****:** Empresas financeiras e departamentos de contabilidade usam SQL para gerenciar e analisar dados financeiros, incluindo transações, balanços e demonstrações financeiras.

Como você pode perceber, atualmente, os dados desempenham um papel fundamental na nossa sociedade. No fim das contas, todos nós somos pessoas que consomem e, ativamente, produzem dados.

Assim, em diferentes contextos — da economia, da ciência, da saúde e da educação, a coleta e a análise de informações são pontos fundamentais para embasar decisões criteriosas e promover avanços significativos.

### Como o SQL funciona? <a href="#como-o-sql-funciona" id="como-o-sql-funciona"></a>

A linguagem SQL serve para interagir com os nossos dados que estão armazenados dentro de um banco de dados relacional, permitindo a criação, a recuperação, a atualização e a exclusão desses dados.

O SQL funciona, portanto, por meio de vários componentes e processos que trabalham juntos para gerenciar bancos de dados relacionais, como:

* Analisador: analisa a instrução SQL, verificando sua sintaxe e convertendo palavras-chave em símbolos especiais.
* Exatidão: garante que a instrução esteja em conformidade com as regras da SQL, como o uso correto de pontos e vírgulas.
* Autorização: valida se o usuário tem permissão para executar a consulta e manipular os dados, com base em suas credenciais.
* Mecanismo Relacional: cria um plano eficiente para acessar os dados, otimizando a consulta e verificando consultas semelhantes.
* Mecanismo de Armazenamento: executa a instrução SQL, lendo e gravando os dados nos arquivos de armazenamento físico.

### Quais são os componentes de um sistema SQL? <a href="#quais-sao-os-componentes-de-um-sistema-sql" id="quais-sao-os-componentes-de-um-sistema-sql"></a>

Um sistema SQL (Sistema de Gerenciamento de Banco de Dados Relacionais) é composto por vários componentes que trabalham juntos para permitir a **criação**, a **manutenção** e a **consulta** de bancos de dados relacionais:

* Tabelas: as tabelas são estruturas fundamentais em um banco de dados SQL. Elas são usadas para armazenar os dados em formato tabular, com colunas representando atributos e linhas representando registros. As tabelas são projetadas com base no modelo relacional.
* Procedimentos Armazenados: os procedimentos armazenados são blocos de código SQL que podem ser definidos e armazenados no banco de dados. Eles são usados para realizar tarefas específicas, como cálculos complexos, validações de dados e processamento personalizado. Os procedimentos armazenados podem ser chamados por aplicativos ou outros procedimentos armazenados.
* Instruções SQL: As instruções SQL, como SELECT, INSERT, UPDATE e DELETE, são usadas para manipular os dados em tabelas. As instruções SQL permitem que os usuários realizem consultas, insiram novos dados, atualizem registros existentes e excluam dados.

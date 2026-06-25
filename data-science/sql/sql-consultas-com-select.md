# SQL: consultas com SELECT

O banco de dados não é utilizado apenas para armazenar dados, em alguns momentos é necessário realizar consultas ou até comunicar com outras ferramentas e sistemas para fazer relatórios, mas como tudo isso é feito? Com a **cláusula SELECT**, a principal função do `SELECT` é consultar/buscar os dados de uma tabela em um banco de dados.

Vamos supor que precisamos desenvolver um relatório para algum cliente/sistema ou até mesmo consultar uma informação que mostra inconsistência no sistema, mas como fazemos isso? Usando o SELECT para buscar esses dados

## SELECT SIMPLES <a href="#select-simples" id="select-simples"></a>

A sintaxe básica do comando é:

{% code lineNumbers="true" fullWidth="false" %}
```sql
SELECT campos FROM nome_da_tabela
```
{% endcode %}

Bom, nós temos em nosso banco de dados a tabela de clientes:

| id | nome             | telefone  | genero | data\_cadastro |
| -- | ---------------- | --------- | ------ | -------------- |
| 1  | Roberta de Jesus | 9999-9999 | F      | 10-12-2019     |
| 2  | Denis Oliveira   | 8888-8888 | M      | 05-11-2019     |
| 3  | Valeria Custodio | 7777-7777 | F      | 20-10-2019     |
| 4  | Isabel Borges    | 5555-555  | F      | 01-10-2019     |

Então, para consultar podemos fazer de duas formas:

* Trazendo todos os campos:

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes
```
{% endcode %}

Neste comando, todos clientes são retornados.

Ou

* Trazendo alguns campos:

{% code lineNumbers="true" %}
```sql
SELECT nome, telefone FROM clientes
```
{% endcode %}

Neste comando, nome e telefone de todos os clientes são retornados.

O **asterisco** representa todos os campos. É bem prático, mas não muito utilizado. Ao usar o **asterisco** para trazer todos os campos, ele obriga o servidor do banco de dados a procurar os campos antes de trazer os dados e assim, causando uma demora no retorno da consulta.

## SELECT COM WHERE <a href="#select-com-where" id="select-com-where"></a>

O `WHERE` é utilizado no SQL para passar condições/regras de filtragem.

Vamos supor que queremos ver todos clientes do gênero feminino em nossa tabela, então, utilizamos o WHERE para realizar essa filtragem:

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes 
         WHERE genero = “F”
```
{% endcode %}

Neste comando, todos clientes do gênero feminino são retornados.

Também podemos usar **operadores lógicos** para usar mais de uma condição dentro do WHERE.

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes 
         WHERE genero = “F” AND nome LIKE “R%”
```
{% endcode %}

Neste comando, todos os clientes do gênero feminino com nomes que iniciam com **R** serão retornados.

Além do operador de igual (=) temos o **IN** e o **BETWEEN**.

* **IN**

O IN é utilizado para fazer a filtragem a partir de uma lista de buscas.

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes 
         WHERE genero IN id (1, 2, 3)
```
{% endcode %}

Neste comando, todos os clientes com id 1, 2 e 3 serão retornados.

* **BETWEEN**

O BETWEEN é utilizado para fazer buscas entre intervalos. É mais utilizado para filtrar intervalos de datas.

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes 
         WHERE data_cadastro BETWEEN ‘10-12-2019’ AND ‘20-10-2019’
```
{% endcode %}

Neste comando, todos os clientes que foram cadastrados entre essas datas serão retornados.

## LIKE <a href="#like" id="like"></a>

O LIKE é utilizado para buscar **strings(texto)** dentro de uma coluna com valores textuais. Podemos buscar as linhas que o **nome** inicia com uma determinada palavra, como vimos acima ou contém um certo texto.

* `string`: são retornadas todas as linhas que tem na coluna buscada exatamente a "string" informada no filtro. É a mesma coisa de usar o operador de igual.
* `%string%`: são retornadas as linhas que tem na coluna buscada a "string" informada. Podemos buscar os nomes que tem "Jesus", ou que tem alguma sílaba ou letra específica. A linha com o nome "Roberta de Jesus", contém o termo "da", então atenderia ao filtro '%de%'.
* `%string`: são retornadas as linhas que a coluna filtrada termina com a "string" informada. O % indica que pode ter qualquer valor no começo do campo, desde que ele termine com a “string". A linha com nome "Roberta de Jesus" atenderia ao filtro '%Jesus'.
* `string%`: são retornadas as linhas que o coluna filtrada começa com a “string" informada. O % indica que depois da “string” pode ter qualquer valor. A linha com nome "Roberta de Jesus", atenderia ao filtro 'Roberta%'.

## ORDER BY <a href="#order-by" id="order-by"></a>

O ORDER BY é utilizado para ordenação. Podemos ordenar em ordem crescente (ASC) ou em ordem decrescente (DESC).

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes 
         ORDER BY nome ASC
```
{% endcode %}

Neste comando, serão retornados todos os clientes ordenados pelo nome em ordem crescente.

Então pessoal, vimos neste artigo como fazer consultas simples apenas com o SELECT, como incrementar essas consultas criando filtros com WHERE e ordenar os dados com o ORDER BY.

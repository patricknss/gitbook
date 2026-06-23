# SQL: consultas com SELECT

O banco de dados não é utilizado apenas para armazenar dados, em alguns momentos é necessário realizar consultas ou até comunicar com outras ferramentas e sistemas para fazer relatórios, mas como tudo isso é feito? Com a <mark style="color:blue;">**cláusula SELECT**</mark>, a principal função do <mark style="color:green;">`SELECT`</mark> é consultar/buscar os dados de uma tabela em um banco de dados.

Vamos supor que precisamos desenvolver um relatório para algum cliente/sistema ou até mesmo consultar uma informação que mostra inconsistência no sistema, mas como fazemos isso? Usando o SELECT para buscar esses dados

## SELECT SIMPLES <a href="#select-simples" id="select-simples"></a>

<mark style="color:yellow;">A sintaxe básica do comando é:</mark>

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

* <mark style="color:yellow;">Trazendo todos os campos:</mark>

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes
```
{% endcode %}

Neste comando, todos clientes são retornados.

Ou

* <mark style="color:yellow;">Trazendo alguns campos:</mark>

{% code lineNumbers="true" %}
```sql
SELECT nome, telefone FROM clientes
```
{% endcode %}

Neste comando, nome e telefone de todos os clientes são retornados.

O <mark style="color:blue;">**asterisco**</mark> representa todos os campos. É bem prático, mas não muito utilizado. Ao usar o **asterisco** para trazer todos os campos, ele obriga o servidor do banco de dados a procurar os campos antes de trazer os dados e assim, causando uma demora no retorno da consulta.

## SELECT COM WHERE <a href="#select-com-where" id="select-com-where"></a>

O <mark style="color:green;">`WHERE`</mark> é utilizado no SQL para passar condições/regras de filtragem.

Vamos supor que queremos ver todos clientes do gênero feminino em nossa tabela, então, utilizamos o <mark style="color:green;">WHERE</mark> para realizar essa filtragem:

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes 
         WHERE genero = “F”
```
{% endcode %}

Neste comando, todos clientes do gênero feminino são retornados.

Também podemos usar **operadores lógicos** para usar mais de uma condição dentro do <mark style="color:green;">WHERE</mark>.

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes 
         WHERE genero = “F” AND nome LIKE “R%”
```
{% endcode %}

Neste comando, todos os clientes do gênero feminino com nomes que iniciam com <mark style="color:yellow;">**R**</mark> serão retornados.

Além do operador de igual (=) temos o <mark style="color:green;">**IN**</mark> e o <mark style="color:green;">**BETWEEN**</mark>.

* <mark style="color:blue;">**IN**</mark>

O IN é utilizado para fazer a filtragem a partir de uma lista de buscas.

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes 
         WHERE genero IN id (1, 2, 3)
```
{% endcode %}

Neste comando, todos os clientes com id 1, 2 e 3 serão retornados.

* <mark style="color:blue;">**BETWEEN**</mark>

O BETWEEN é utilizado para fazer buscas entre intervalos. É mais utilizado para filtrar intervalos de datas.

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes 
         WHERE data_cadastro BETWEEN ‘10-12-2019’ AND ‘20-10-2019’
```
{% endcode %}

Neste comando, todos os clientes que foram cadastrados entre essas datas serão retornados.

## LIKE <a href="#like" id="like"></a>

O <mark style="color:blue;">LIKE</mark> é utilizado para buscar <mark style="color:yellow;">**strings(texto)**</mark> dentro de uma coluna com valores textuais. Podemos buscar as linhas que o **nome** inicia com uma determinada palavra, como vimos acima ou contém um certo texto.

* <mark style="color:green;">`string`</mark>: são retornadas todas as linhas que tem na coluna buscada exatamente a "string" informada no filtro. <mark style="color:yellow;">É a mesma coisa de usar o operador de igual.</mark>
* <mark style="color:green;">`%string%`</mark>: são retornadas as linhas que tem na coluna buscada a "string" <mark style="color:yellow;">informada.</mark> Podemos buscar os nomes que tem "Jesus", ou que tem alguma sílaba ou letra específica. A linha com o nome "Roberta de Jesus", contém o termo "da", então atenderia ao filtro '%de%'.
* <mark style="color:green;">`%string`</mark>: são retornadas as linhas que a coluna filtrada <mark style="color:yellow;">termina com</mark> a "string" informada. O % indica que pode ter qualquer valor no começo do campo, desde que ele termine com a “string". A linha com nome "Roberta de Jesus" atenderia ao filtro '%Jesus'.
* <mark style="color:green;">`string%`</mark>: são retornadas as linhas que o coluna filtrada <mark style="color:yellow;">começa com</mark> a “string" informada. O % indica que depois da “string” pode ter qualquer valor. A linha com nome "Roberta de Jesus", atenderia ao filtro 'Roberta%'.

## ORDER BY <a href="#order-by" id="order-by"></a>

O <mark style="color:blue;">ORDER BY</mark> é utilizado para ordenação. Podemos ordenar em ordem crescente (<mark style="color:orange;">ASC</mark>) ou em ordem decrescente (<mark style="color:orange;">DESC</mark>).

{% code lineNumbers="true" %}
```sql
SELECT * FROM clientes 
         ORDER BY nome ASC
```
{% endcode %}

Neste comando, serão retornados todos os clientes ordenados pelo nome em ordem crescente.

Então pessoal, vimos neste artigo como fazer consultas simples apenas com o <mark style="color:green;">SELECT</mark>, como incrementar essas consultas criando filtros com <mark style="color:green;">WHERE</mark> e ordenar os dados com o <mark style="color:green;">ORDER BY</mark>.

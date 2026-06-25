---
description: >-
  Após a finalização de um projeto de modelagem de dados e a sua implementação,
  inicia-se o desenvolvimento para a manutenção do banco de dados.
---

# SQL: cláusulas UPDATE e DELETE

Para alterar e excluir dados de uma tabela, utilizamos duas cláusulas bem importantes da **linguagem SQL**: a cláusula <mark style="color:#2E7D32;">`UPDATE`</mark> que é responsável por alterar os dados armazenados e a cláusula <mark style="color:#2E7D32;">`DELETE`</mark> que é responsável por remover os dados.

Neste artigo, vamos conhecer a sintaxe básica de cada cláusula e entender quais os riscos e os cuidados necessários ao executar as cláusulas <mark style="color:#2E7D32;">`UPDATE`</mark> e <mark style="color:#2E7D32;">`DELETE`</mark>.

## Cláusula UPDATE

Utilizamos a cláusula <mark style="color:#2E7D32;">`UPDATE`</mark> para realizar alterações nos dados armazenados em uma tabela do banco de dados. A sintaxe básica do comando utilizado por todos os bancos de dados relacionais é:

{% code lineNumbers="true" %}
```sql
UPDATE nome_da_tabela
SET coluna = valor, 
WHERE condição;
```
{% endcode %}

Na cláusula `UPDATE`, informamos o nome da tabela que queremos atualizar, utilizamos o <mark style="color:#2E7D32;">`SET`</mark> para indicar os campos da tabela que serão atualizados e no <mark style="color:#2E7D32;">`WHERE`</mark> expressamos a condição para a atualização, ou seja, especificamos quais registros devem ser atualizados na tabela.

## Cláusula DELETE <a href="#clausula-delete" id="clausula-delete"></a>

Utilizamos a cláusula <mark style="color:#2E7D32;">`DELETE`</mark> para realizar a exclusão de dados de uma ou mais tabelas de um banco de dados. A sintaxe básica do comando utilizado por todos os bancos de dados relacionais é:

{% code lineNumbers="true" %}
```sql
DELETE
FROM nome_da_tabela, 
WHERE condição;
```
{% endcode %}

Onde na cláusula <mark style="color:#2E7D32;">`FROM`</mark>, informamos o nome da tabela que queremos excluir os dados, e no <mark style="color:#2E7D32;">`WHERE`</mark> informamos a condição que especifica quais registros devem ser excluídos da tabela.

## Riscos e Cuidados ao executar os comandos <a href="#riscos-e-cuidados-ao-executar-os-comandos" id="riscos-e-cuidados-ao-executar-os-comandos"></a>

Ao executar os comandos para realizar a atualização ou exclusão de dados de uma tabela de um banco de dados, precisamos tomar alguns cuidados.

Nesse sentido, um ponto de atenção é sempre informar uma condição ao realizar uma atualização ou exclusão de dados de uma tabela. Quando não informamos essa condição, corremos o risco de que todos os dados da tabela sejam atualizados, ou até mesmo excluídos, ocorrendo assim a perda de dados importantes.

## Cláusula WHERE <a href="#clausula-where" id="clausula-where"></a>

Para que essa perda de dados em massa não ocorra, utilizamos a cláusula ´WHERE´. Especificamos nesta cláusula os critérios que os dados armazenados em uma tabela devem cumprir para que os registros que contêm esses parâmetros sejam incluídos nos resultados da consulta.

Para aplicarmos na prática a utilização das cláusulas <mark style="color:#2E7D32;">`DELETE`</mark> e 'UPDATE'. Vamos observar o seguinte exemplo:

Em um banco de dados existe a tabela de clientes e a tabela de vendedores, com os seguintes campos e registros:

| ID | CPF         | NOME                | ENDEREÇO              | BAIRRO      | CIDADE         | ESTADO | CEP      |
| -- | ----------- | ------------------- | --------------------- | ----------- | -------------- | ------ | -------- |
| 01 | 1471156710  | Érica Carvalho      | R. Iriquitia          | Jardins     | São Paulo      | SP     | 80012212 |
| 02 | 19290992743 | Fernando Cavalcante | R. Dois de Fevereiro  | Agua Santa  | Rio de Janeiro | RJ     | 22000000 |
| 03 | 2600586709  | César Teixeira      | Rua Conde de Bonfim   | Tijuca      | Rio de Janeiro | RJ     | 22020001 |
| 04 | 492472718   | Eduardo Jorge       | R. Volta Grande       | Tijuca      | Rio de Janeiro | RJ     | 22012002 |
| 05 | 50534475787 | Abel Silva          | Rua Humaitá           | Humaitá     | Rio de Janeiro | RJ     | 22000212 |
| 06 | 5576228758  | Petra Oliveira      | R. Benício de Abreu   | Lapa        | São Paulo      | SP     | 88192029 |
| 07 | 5840119709  | Gabriel Araujo      | R. Manuel de Oliveira | Santo Amaro | São Paulo      | SP     | 80010221 |

***

| MATRÍCULA | NOME            | BAIRRO      | COMISSÃO | DATA ADMISSÃO |
| --------- | --------------- | ----------- | -------- | ------------- |
| 235       | Márcio Almeida  | Tijuca      | 0.08     | 2014-08-15    |
| 236       | Cláudia Morais  | Jardins     | 0.08     | 2013-09-17    |
| 237       | Roberta Martins | Copacabana  | 0.11     | 2017-03-18    |
| 238       | Péricles Alves  | Santo Amaro | 0.11     | 2016-08-21    |

Estas duas tabelas, serão utilizadas em todos os exemplos apresentados durante o artigo.

## UPDATE Com WHERE <a href="#update-com-where" id="update-com-where"></a>

Tendo então a tabela de clientes acima como nosso exemplo, precisamos atualizar as informações da cliente que possui o CPF \`147115670´ e está na primeira linha da nossa tabela, para isso vamos utilizar a seguinte consulta:

{% code lineNumbers="true" %}
```sql
UPDATE CLIENTES
SET nome = 'Érica Silvia'
WHERE CPF = '1471156710';
```
{% endcode %}

Ao executar este comando apenas a cliente que possui o CPF \`147115670´, terá os seus dados alterados:

| ID | CPF            | NOME                | ENDEREÇO              | BAIRRO      | CIDADE         | ESTADO | CEP      |
| -- | -------------- | ------------------- | --------------------- | ----------- | -------------- | ------ | -------- |
| 01 | **1471156710** | **Érica Silva**     | R. Iriquitia          | Jardins     | São Paulo      | SP     | 80012212 |
| 02 | 19290992743    | Fernando Cavalcante | R. Dois de Fevereiro  | Agua Santa  | Rio de Janeiro | RJ     | 22000000 |
| 03 | 2600586709     | César Teixeira      | Rua Conde de Bonfim   | Tijuca      | Rio de Janeiro | RJ     | 22020001 |
| 04 | 492472718      | Eduardo Jorge       | R. Volta Grande       | Tijuca      | Rio de Janeiro | RJ     | 22012002 |
| 05 | 50534475787    | Abel Silva          | Rua Humaitá           | Humaitá     | Rio de Janeiro | RJ     | 22000212 |
| 06 | 5576228758     | Petra Oliveira      | R. Benício de Abreu   | Lapa        | São Paulo      | SP     | 88192029 |
| 07 | 5840119709     | Gabriel Araujo      | R. Manuel de Oliveira | Santo Amaro | São Paulo      | SP     | 80010221 |

Para atualizar mais de um campo ao mesmo tempo, podemos utilizar a seguinte consulta:

{% code lineNumbers="true" %}
```sql
UPDATE CLIENTES
SET NOME = 'Fernando Sousa',CEP = '80012212'
WHERE CPF = '19290992743';
```
{% endcode %}

Assim, apenas as informações de nome e CEP do segundo cliente que possui o CPF `19290992743` será atualizado:

| ID | CPF         | NOME               | ENDEREÇO              | BAIRRO      | CIDADE         | ESTADO | CEP          |
| -- | ----------- | ------------------ | --------------------- | ----------- | -------------- | ------ | ------------ |
| 01 | 1471156710  | Érica Silva        | R. Iriquitia          | Jardins     | São Paulo      | SP     | 80012212     |
| 02 | 19290992743 | **Fernando Sousa** | R. Dois de Fevereiro  | Agua Santa  | Rio de Janeiro | RJ     | **80012212** |
| 03 | 2600586709  | César Teixeira     | Rua Conde de Bonfim   | Tijuca      | Rio de Janeiro | RJ     | 22020001     |
| 04 | 492472718   | Eduardo Jorge      | R. Volta Grande       | Tijuca      | Rio de Janeiro | RJ     | 22012002     |
| 05 | 50534475787 | Abel Silva         | Rua Humaitá           | Humaitá     | Rio de Janeiro | RJ     | 22000212     |
| 06 | 5576228758  | Petra Oliveira     | R. Benício de Abreu   | Lapa        | São Paulo      | SP     | 88192029     |
| 07 | 5840119709  | Gabriel Araujo     | R. Manuel de Oliveira | Santo Amaro | São Paulo      | SP     | 80010221     |

## UPDATE sem WHERE <a href="#update-sem-where" id="update-sem-where"></a>

Precisamos agora alterar os dados de um outro registro armazenado na nossa tabela de clientes:

{% code lineNumbers="true" %}
```sql
UPDATE Clientes
SET ESTADO = 'SP';
```
{% endcode %}

Como a cláusula <mark style="color:#2E7D32;">`WHERE`</mark> não foi utilizada, informando o registro que deveria ser atualizado, o valor do campo**ESTADO** de todos clientes foram alterados:

| ID | CPF         | NOME           | ENDEREÇO              | BAIRRO      | CIDADE         | ESTADO | CEP      |
| -- | ----------- | -------------- | --------------------- | ----------- | -------------- | ------ | -------- |
| 01 | 1471156710  | Érica Silva    | R. Iriquitia          | Jardins     | São Paulo      | **SP** | 80012212 |
| 02 | 19290992743 | Fernando Sousa | R. Dois de Fevereiro  | Agua Santa  | Rio de Janeiro | **SP** | 80012212 |
| 03 | 2600586709  | César Teixeira | Rua Conde de Bonfim   | Tijuca      | Rio de Janeiro | **SP** | 22020001 |
| 04 | 492472718   | Eduardo Jorge  | R. Volta Grande       | Tijuca      | Rio de Janeiro | **SP** | 22012002 |
| 05 | 50534475787 | Abel Silva     | Rua Humaitá           | Humaitá     | Rio de Janeiro | **SP** | 22000212 |
| 06 | 5576228758  | Petra Oliveira | R. Benício de Abreu   | Lapa        | São Paulo      | **SP** | 88192029 |
| 07 | 5840119709  | Gabriel Araujo | R. Manuel de Oliveira | Santo Amaro | São Paulo      | **SP** | 80010221 |

## DELETE Com WHERE <a href="#delete-com-where" id="delete-com-where"></a>

Um dos clientes, solicitou que as suas informações fossem removidas do banco de dados. Para realizar esta exclusão, vamos utilizar a seguinte consulta:

{% code lineNumbers="true" %}
```sql
DELETE
FROM Clientes
WHERE CPF = '5840119709';
```
{% endcode %}

Ao executar este comando apenas o cliente **Gabriel Araujo** que estava localizado na ultima linha da tabela de clientes, foi excluído:

| ID | CPF         | NOME           | ENDEREÇO             | BAIRRO     | CIDADE         | ESTADO | CEP      |
| -- | ----------- | -------------- | -------------------- | ---------- | -------------- | ------ | -------- |
| 01 | 1471156710  | Érica Silva    | R. Iriquitia         | Jardins    | São Paulo      | **SP** | 80012212 |
| 02 | 19290992743 | Fernando Sousa | R. Dois de Fevereiro | Agua Santa | Rio de Janeiro | SP     | 80012212 |
| 03 | 2600586709  | César Teixeira | Rua Conde de Bonfim  | Tijuca     | Rio de Janeiro | SP     | 22020001 |
| 04 | 492472718   | Eduardo Jorge  | R. Volta Grande      | Tijuca     | Rio de Janeiro | SP     | 22012002 |
| 05 | 50534475787 | Abel Silva     | Rua Humaitá          | Humaitá    | Rio de Janeiro | SP     | 22000212 |
| 06 | 5576228758  | Petra Oliveira | R. Benício de Abreu  | Lapa       | São Paulo      | SP     | 88192029 |

## DELETE sem WHERE <a href="#delete-sem-where" id="delete-sem-where"></a>

Pensando que se a cláusula <mark style="color:#2E7D32;">`WHERE`</mark> não fosse utilizada ao executar um comando <mark style="color:#2E7D32;">`DELETE`</mark> para remover algum registro, teríamos como resultado a exclusão de todos os dados armazenados na tabela, ou seja, perdendo os dados de todos os clientes:

{% code lineNumbers="true" %}
```sql
DELETE
FROM Clientes;
```
{% endcode %}

| CPF | NOME | ENDEREÇO | BAIRRO | CIDADE | ESTADO | CEP |
| --- | ---- | -------- | ------ | ------ | ------ | --- |
|     |      |          |        |        |        |     |
|     |      |          |        |        |        |     |
|     |      |          |        |        |        |     |
|     |      |          |        |        |        |     |
|     |      |          |        |        |        |     |

## Atualizar utilizando como condição outras tabelas <a href="#atualizar-utilizando-como-condicao-outras-tabelas" id="atualizar-utilizando-como-condicao-outras-tabelas"></a>

Podemos informar na cláusula <mark style="color:#2E7D32;">`WHERE`</mark> como condição para realizar atualização ou exclusão dos dados, outra consulta, que pode ser utilizada para buscar informações armazenadas na própria tabela ou em **outras tabelas**:

Sendo assim, vamos analisar o exemplo abaixo:

{% code lineNumbers="true" %}
```sql
UPDATE VENDEDORES
SET COMISSÃO  = COMISSÃO + 0.03
WHERE COMISSÃO = (SELECT min(COMISSÃO) FROM VENDEDORES)
```
{% endcode %}

O <mark style="color:#2E7D32;">`SELECT`</mark> passado na cláusula <mark style="color:#2E7D32;">`WHERE`</mark>, retornará apenas o valor da menor comissão armazenada na tabela. Dessa forma, apenas os vendedores que possuem o valor da comissão igual ao valor retornado neste <mark style="color:#2E7D32;">`SELECT`</mark> terão os seus dados alterados e o valor da sua comissão aumentará:

| MATRÍCULA | NOME            | BAIRRO      | COMISSÃO | DATA ADMISSÃO |
| --------- | --------------- | ----------- | -------- | ------------- |
| 235       | Márcio Almeida  | Tijuca      | **0.11** | 2014-08-15    |
| 236       | Cláudia Morais  | Jardins     | **0.11** | 2013-09-17    |
| 237       | Roberta Martins | Copacabana  | 0.11     | 2017-03-18    |
| 238       | Péricles Alves  | Santo Amaro | 0.11     | 2016-08-21    |

## Chave primária <a href="#chave-primaria" id="chave-primaria"></a>

Uma outra forma de garantir que apenas os dados desejados sofram alterações é a criação de uma chave primária na tabela.

**A chave primária, ou Primary key (PK) é o dado que pode ser utilizado como um identificador único de um registro em uma tabela no banco de dados.**

Se definirmos que o campo <mark style="color:#2E7D32;">`CPF`</mark> será a chave primária da tabela de clientes, estamos definindo que este campo receberá apenas valores únicos. Ao utilizarmos este campo como uma condição no momento de executar uma consulta com as cláusulas <mark style="color:#2E7D32;">`DELETE`</mark> ou <mark style="color:#2E7D32;">`UPDATE`</mark>, garantimos que apenas o registro que possui aquele dado será alterado.

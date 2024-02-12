# Documentacao_SQL
Documentação dos estudos de SQL

### COMANDOS BÁSICOS

* __SELECT__

-> Comando usado para selecionar colunas de tabelas

-> Quando selecionar mais de uma coluna, elas devem ser separadas por vírgula sem conter vírgula antes do comando __FROM__

-> Pode-se utilizar o asterisco (*) para selecionar todas as colunas da tabela

SINTAXE:
~~~sql
select coluna_1, coluna_2, coluna_3
from schema_1.tabela_1
# ordem de consulta nome_banco.nome_tabela
~~~
EXEMPLOS:
1. Liste os e-mails dos clientes da tabela sales.customers
~~~sql
select email
from sales.customers
~~~
2. Liste os e-mails e o nome dos clientes da tabela sales.customers
~~~sql
select email, first_name, last_name
from sales.customers
~~~
3. Liste todas as informações dos clientes da tabela sales.customers
~~~sql
select *
from sales.customers
~~~

* __DISTINCT__

-> Comando usado para remover linhas duplicadas e mostrar apenas linhas distintas

-> Muito utilizado na etapa de exploração dos dados

-> Caso mais de uma coluna seja selecionada, o comando **SELECT DISTINCT** irá retornar todas as combinações distintas

SINTAXE:
~~~sql
select distinct coluna_1, coluna_2, coluna_3
from schema_1.tabela_1
~~~
EXEMPLOS:
1. Seleção de uma coluna sem DISTINCT -- Liste as marcas de carro que constam na tabela products
~~~sql
select brand
from sales.products
~~~
2. Seleção de uma coluna com DISTINCT -- Liste as marcas de carro distintas que constam na tabela products
~~~sql
select distinct brand
from sales.products
~~~
3. Seleção de mais de uma coluna com DISTINCT -- Liste as marcas e anos de modelo distintos que constam na tabela products
~~~sql
select distinct brand, model_year
from sales.products
~~~

* __WHERE__

-> Comando utilizado para filtrar linhas de acordo com uma condição

-> No PostgreSQL são utilizadas aspas simples para delimitar strings

-> String = sequência de caracteres = texto

-> Pode-se combinar mais de uma condição utilizando os operadores lógicos

-> No PostgreSQL as são escritas no formato 'YYYY-MM-DD' ou 'YYYYMMDD'

SINTAXE:
~~~sql
select coluna_1, coluna_2, coluna_3
from schema_1.tabela_1
where condição_x
~~~
EXEMPLOS:
1. Filtro com condição única -- Liste os emails dos clientes da nossa base que moram no estado de Santa Catarina
~~~sql
select emais, state
from sales.customers
where state='SC'
~~~
2. Filtro com mais de uma condição -- Liste os emails dos clientes de Santa Catarina ou Mato Grosso do Sul
~~~sql
select email, state
from sales.customers
where state='SC' or state='MS'
~~~
3. Condições com datas -- Liste os emails dos clientes que moram em Santa Catarina ou Mato Grosso do Sul e que tem mais de 30 anos
~~~sql
select email, state, birth_date
from sales.customers
where (state='SC'or state='MS') and birth_date < '1994-02-03'
~~~

* __ORDER BY__

-> Comando utilizado para ordenar a seleção de acordo com uma regra definida

-> Por padrão o comando ordena na ordem crescente. Para mudar para a ordem decrescente usa-se o comando **desc**

-> No caso de strings a ordenação será seguida na ordem alfabética

SINTAXE:
~~~sql
select coluna_1, coluna_2, coluna_3
from schema.tabela_1
where condição_x = True
order by coluna_1
~~~
EXEMPLOS:
1. Ordenação de valores numéricos -- Liste produtos da tabela products na ordem crescente com base no preço
~~~sql
select *
from sales.products
order by price

select *
from sales.products
order by price desc # Caso queira na ordem decrescente
~~~

2. Ordenação de texte -- Liste os estados distintos da tabela customers na ordem crescente
~~~sql
select distinct state
from sales.customers
order by state
~~~

* __LIMIT__

-> Comando utilizado para limitar o nº de linhas da consulta

-> Muito utilizado na etapa de exploração dos dados

-> Muito utilizado em conjunto com o comando **order by** quando o que importa são os TOP N. Ex.: "N pagamentos mais recentes", "N produtos mais caros"

SINTAXE:
~~~sql
select coluna_1, coluna_2, coluna_3
from schema_1.tabela_1
where condição = True
order by coluna_1
limit N
~~~
EXEMPLOS:
1. Seleção das N primeiras linhas usando LIMIT -- Liste as 10 primeiras linhas da tabela funnel
~~~sql
select *
from sales.funnel
limit 10
~~~
2. Seleção das N primeiras linhas usando LIMIT e ORDER BY -- Liste os 10 produtos mais caros da tabela products
~~~sql
select *
from sales.products
order by price desc
limit 10
~~~ 
##### EXEMPLOS DE CONSULTAS

1. Selecione os nomes de cidade distintas que existem no estado de Minas Gerais em ordem alfabética (dados da tabela sales.customers)
~~~sql
select distinct city
from sales.customers
where state = "MG"
order by city
~~~
2. Selecione o visit_id das 10 compras mais recentes efetuadas (dados da tabela sales.funnel)
~~~sql
select visit_id
from sales.funnel
where paid is not null
order by paid_date desc
limit 10
~~~
3. Selecione todos os dados dos 10 clientes com maior score nascidos após 01/01/2000 (dados da tabela sales.customers)
~~~sql
select *
from sales.customers
where birth_date >= '2000-01-01'
order by score desc
limit 10
~~~
### OPERADORES

* __ARITMÉTICOS__

-> Servem para executar operações matemáticas

-> Muito utilizado para criar colunas calculadas

-> Alias(pseudônimos) são muito utilizados para dar nome as colunas calculadas

-> Para criar pseudônimos que contém espaços no nome são utilizadas aspas duplas

-> No caso de strings o operador de adição (||) irá concatenar as strings 


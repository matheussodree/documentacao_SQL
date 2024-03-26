# Documentacao_SQL
Documentação dos estudos de SQL

Dialeto do SQL -> PostgreSQL 

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

TIPOS:
~~~sql
+ (adição) , - (subtração) , * (multiplicação) , / (divisão) , ^ (exponenciação) ,

% (módulo) e ||(operador de concatenação)
~~~
EXEMPLOS:
1. Criação de coluna calculada -- Crie uma coluna contendo a idade do cliente da tabela sales.customers
~~~sql
select
  email,
  birth_date
  (current_date - birth_date) / 365 as idade_do_cliente
from sales.customers

select
  email,
  birth_date
  (current_date - birth_date) / 365 as "idade do cliente"
from sales.customers
~~~
2. Utilização da coluna calculada nas queries -- Liste os 10 clientes mais novos da tabela customers
~~~sql
select
    email,
    birth_date
    (current_date - birth_date) / 365 as "Idade do cliente"
from sales.customers
order by "Idade do cliente"
~~~
3. Criação de coluna calculada com strings -- Crie a coluna "nome_completo" contendo o nome completo do cliente
~~~sql
select
      first_name || ' ' || last_name as nome_completo
from sales.customers
~~~
* __COMPARAÇÃO__

-> Servem para comparar dois valores retornando TRUE ou FALSE

-> Muito utilizado em conjunto com a função WHERE para filtrar linhas de uma seleção

-> Utilizados para criar colunas Flag que retornem TRUE ou FALSE

TIPOS:
~~~sql
= (igual), > (maior que), < (menor que), >= (maior ou igual), <= (menor ou igual)
~~~
EXEMPLOS:
1. Uso de operadores como flag -- Crie uma coluna que retorne TRUE sempre que um cliente for um profissional clt
~~~sql
select
    customer_id,
    first_name,
    professional_status,
    (professional_status = 'clt') as cliente_clt
from sales.customers
~~~
* __LÓGICO__

-> Usados para unir expressões simples em uma composta

-> AND: verifica se duas comparações são simultaneamente verdadeiras

-> OR: verifica se uma ou outra comparação é verdadeira

-> BETWEEN: verifica quais valores estão dentro do range definido

-> IN: funciona como múltiplos ORs

-> LIKE e ILIKE: comparam textos e são sempre utilizados em conjunto com o operador %, que funciona como um coringa, indicando que qualquer texto pode aparecer no lugar do campo

-> ILIKE: ignora se o campo tem letras maiúsculas ou minúsculas na comparação

-> IS NULL: verifica se o campo é nulo

EXEMPLOS:

1. Uso do comando BETWEEN -- Selecione veículos que custam entre 100k e 200k na tabela products
~~~sql
select *
from sales.products
where price between 100000 and 200000
~~~
2. Uso do comando NOT -- Selecione veículos que custam abaixo de 100k ou acima de 200k
~~~sql
select *
from sales.products
where price not between 100000 and 200000
~~~
3. Uso do comando IN -- Selecione produtos que sejam da marca Honda, Toyota ou Renault
~~~sql
select *
from sales.products
where brand not in('HONDA', 'TOYOTA', 'RENAULT')
~~~
4. Uso do comando LIKE (matchs imperfeitos) -- Selecione os primeiros nomes distintos da tabela customers que começam com as iniciais ANA
~~~sql
select distinct first_name
from sales.customers
where first_name like 'ANA%'
~~~
5. Uso do comando ILIKE(ignora letras maiúsculas e minúsculas) -- Selecione os primeiros nomes distintos com iniciais 'ana'
~~~sql
select distinct first_name
from sales.customers
where first_name ilike 'ana%'
~~~
6. Uso do comando IS NULL -- Selecionar apenas as linhas que contém nulo no campo 'population' na tabela temp_tables.regions
~~~sql
select *
from temp_tables.regions
where population is null
~~~
##### EXEMPLOS DE OPERADORES
1. Calcule quantos salários mínimos ganha cada cliente da tabela sales.customers -- Selecione as colunas de: email, income e a coluna calculada "salários mínimos" -- considere o salário mínimo igual à R$ 1400
~~~sql
select
      email,
      income,
      (income) / 1200 as "salários mínimos"
from sales.customers
~~~
2. Na query anterior acrescente uma coluna informando TRUE se o cliente ganha acima de 5 salários mínimos e FALSE se ganha 4 salários ou menos -- Chame a nova coluna de "acima de 4 salários "
~~~sql
select email
       income
       (income) / 1200 as "salários mínimos"
       ((income) / 1200 ) > 4 as "acima de 4 salários"
from sales.customers
~~~
3. Na query anterior filtre apenas os clientes que ganham entre 4 e 5 salários mínimos -- Utilize o comando BETWEEN
~~~sql
select email
       income
       (income) / 1200 as "salários mínimos"
       ((income) / 1200 ) > 4 as "acima de 4 salários"
from sales.customers
where ((income) / 1200 ) between 4 and 5
~~~
4. Selecione o email, cidade e estado dos clientes que moram no estado de Minas Gerais e Mato Grosso
~~~sql
select email, city, state
from sales.customers
where state in ('MT', 'MG')
~~~
5. Selecione o email, cidade e estado dos clientes que não moram no estado de São Paulo
~~~sql
select email, city, state
from sales.customers
where state not in ('SP')
~~~
6. Selecione os nomes das cidades que começam com a letra Z -- Dados da tabela temp_tables.regions
~~~sql
select city
from temp_tables.regions
where city ilike 'z%'
~~~
* **FUNÇÕES AGREGADAS**

-> Servem para executar operações aritméticas nos registros de uma coluna

-> Funções agregadas não computam células vazias (NULL) como zero

-> Na função COUNT() pode-se utilizar o asterisco (*) para contar todos os registros

-> COUNT(DISTINCT) irá contar apenas os valores exclusivos

TIPOS:
~~~sql
count(), sum(), min(), max(), avg()
~~~
EXEMPLOS:
1. Contagem de todas as linhas de uma tabela -- Conte todas as visitas realizadas ao site da empresa
~~~sql
select count(*)
from sales.funnel
~~~
2. Contagem das linhas de uma coluna -- Conte todos os pagamentos registrados na tabela sales.funnel
~~~sql
select count(paid_date)
from sales.funnel
~~~
3. Contagem distinta de uma coluna -- Conte todos os produtos distintos visitados em jan/24
~~~sql
select count(distinct product_id)
from sales.funnel
where visit_page_date between '2024-01-01' and '2024-01-31'
~~~
4. Calcule o preço mínimo, máximo e médio dos produtos da tabela products
~~~sql
select min(price), max(price), avg(price)
from sales.products
~~~
5. Informe qual é o veículo mais caro da tabela products 
~~~sql
select max(price)
from sales.products

select *
from sales.products
where price = (select max(price) from sales.products)
~~~

* __GROUP BY__

-> Serve para agrupar registros semelhantes de uma coluna

-> Normalmente utilizado em conjunto com as funções de agregação

-> Pode-se referenciar a coluna a ser agrupada pela sua posição ordinal (ex.: group by 1,2,3 irá agrupar pelas 3 primeiras colunas da tabela)

-> O group by sozinho funciona como um distinct, eliminando linhas duplicadas

EXEMPLOS:
1. Contagem agrupada de uma coluna -- Calcule o nº de clientes da tabela customers por estado
~~~sql
select state, count(*) as contagem
from sales.customers
group by state
order by contagem desc
~~~
2. Contagem agrupada de várias colunas -- Calcule o nº de clientes por estado e status profissional
~~~sql
select state, professional_status, count(*) as contagem
from sales.customers
group by 1, 2
order by state, contagem desc
~~~
3. Seleção os estados distintos na tabela customers utilizando o group by 
~~~sql
select distinct state
from sales.customers

select state
from sales.customers
group by state
~~~
* __HAVING__

-> Tem a mesma função do WHERE mas pode ser usado para filtrar os resultados das funções agregadas enquanto o WHERE possui essa limitação

-> A função HAVING também pode filtrar colunas não agregadas

EXEMPLOS:
1. Seleção com filtro no HAVING -- Calcule o nº de clientes por estado filtrando apenas estados acima de 100 clientes
~~~sql
select
    state,
    count(*)
from sales.customers
group by state
having count(*) > 100
~~~
##### EXEMPLOS DE FUNÇÕES AGREGADAS
1. Conte quantos clientes da tabela sales.customers tem menos de 30 anos
~~~sql
select count(*)
from sales.customers
where ((current_date - birth_date) / 365) < 30
~~~
2. Informe a idade do cliente mais velho e mais novo da tabela sales.customers
~~~sql
select
      max((current_date - birth_date) / 365),
      min((current_date - birth_date) / 365)
from sales.customers
~~~
3. Selecione todas as informações do cliente mais rico da tabela sales.customers
~~~sql
select *
from sales.customers
where income = (select max(income) from sales.customers)
~~~
4. Conte quantos veículos de cada marca tem registrado na tabela sales.products -- Ordene o resultado pelo nome da marca
~~~sql
select brand, count(*)
from sales.products
group by brand
order by brand
~~~
5. Conte quantos veículos existem registrados na tabela sales.products -- Por marca e ano do modelo -- Ordene pela nome da marca e pelo ano do veículo
~~~sql
select brand, model_year, count(*)
from sales.products
group by brand, model_year
order by brand, model_year
~~~
6. Conte quantos veículos de cada marca tem registrado na tabela sales.products -- Mostre apenas as marcas que contém mais de 10 veículos registrados
~~~sql
select brand, count(*)
from sales.products
group by brand
having count(*) > 10
~~~

* __JOINS__

-> Servem para combinar colunas de uma ou mais tabelas

-> Pode-se chamar todas as colunas com o asterisco (*), mas não é recomendado

-> É uma boa prática criar alises para nomear as tabelas utilizadas

-> O JOIN sozinho funciona como INNER JOIN

SINTAXE: 
~~~sql
select t1.coluna_1, t1.coluna_1, t2.coluna_1, t2.coluna_2
from schema.tabela_1 as t1 join schema.tabela_2 as t2 on condição_de_join
~~~
EXEMPLOS: 
1. Utilize o LEFT JOIN para fazer join entre as tabelas temp_tables.tabela_1 e temp_tables.tabela_2
~~~sql
select * from temp_tables.tabela_1
select * from temp_tables.tabela_2

select t1.cpf, t1.name, t2.state
from temp_tables.tabela_1 as t1
left join temp_tables.tabela_2 as t2 on t1.cpf = t2.cpf
~~~
2. Utilize o INNER JOIN para fazer join entre as tabelas -- temp_tables.tabela_1 e temp_tables.tabela_2
~~~sql
select t1.cpf, t1.name, t2.state
from temp_tables.tabela_1 as t1
inner join temp_tables.tabela_2 as t2 on t1.cpf = t2.cpf
~~~
3. Utilize o RIGHT JOIN para fazer join entre as tabelas -- temp_tables.tabela_1 e temp_tables.tabela_2
~~~sql
select t2.cpf, t1.name, t2.state
from temp_tables.tabela_1 as t1
right join temp_tables.tabela_2 as t2 on t1.cpf = t2.cpf
~~~
4. Utilize o FULL JOIN para fazer join entre as tabelas -- temp.tables.tabela_1
e temp_tables.tabela_2
~~~sql
select t2.cpf, t1.name, t2.state
from temp_tables.tabela_1 as t1
full join temp_tables.tabela_2 as t2 on t1.cpf = t2.cpf
~~~
#### EXEMPLOS DE JOINS
1. Identifique qual é o status profissional mais frequente nos clientes que compraram automóveis no site
~~~sql
select
      cus.professional_status,
      count(fun.paid_date) as pagamentos
from sales.funnel as fun
left join sales.customers as cus
        on fun.customers_id = cus.customers_id
group by cus.professional_status
order by pagamentos desc
~~~
2. Identifique qual é o gênero mais frequente nos clientes que compraram automóveis no site -- OBS: utilizar a tabela temp_tables.ibge_genders
3. Identifique de quais regiões são os clientes que mais visitam o site -- OBS: utilizar a tabela temp_tables.regions

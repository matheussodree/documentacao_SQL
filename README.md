# Documentacao_SQL
Documentação dos estudos de SQL

### COMANDOS BÁSICOS

* SELECT

-> Comando usado para selecionar colunas de tabelas

-> Quando selecionar mais de uma coluna, elas devem ser separadas por vírgula sem conter vírgula antes do comando __FROM__

-> Pode-se utilizar o asterisco (*) para selecionar todas as colunas da tabela

__SINTAXE__:
~~~sql
select coluna_1, coluna_2, coluna_3
from schema_1.tabela_1
# ordem de consulta nome_banco.nome_tabela
~~~
__EXERCÍCIOS__:
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

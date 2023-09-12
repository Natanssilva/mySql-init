# mySql-init
init study for MySQL

- banco de dados são conjuntos de tabelas e tabelas são conjuntos de registros.
   - tabelas possuem registros de caractristicas iguais com valores diferentes.
   - TABELAS SÃO COMPOSTAS POR CAMPOS, por exemplo: numa TABELA pessoas, possui os campos: peso,sexo,altura,nacionalidade, nome
   - esses CAMPOS devem ser atribuidos a um TIPO PRIMITIVO, como em familias : DATA/TEMPO,NUMÉRICOS.LITERAIS E ESPACIAIS. Mas essas famílias possuem subdivisões e subtipos
   - NÚMERICOS:
      - INTEIROS -> tinyINT, smallINT, int, mediumINT e bigINT (diferença entre eles é a quantidade de bytes que vão usar)
   - REAIS -> decimal, float, double , real
   - LÓGICOS -> bit, boolean
   - DATA/TEMPO -> date/DateTime/ TimeStamp, Time, Year
   - LITERAL:
     - CARACTERES -> char, varchar //char é fixo e varchar é adaptivo conforme o tamanho 
     - BINÁRIOS -> tinyBlob, blob, mediumBlob e LongBlob
     - TEXTO -> tinyText, text, mediumText, LongText  //diferença de caractere e texto: texto são textos longos, por exemplo se quiser guardar um texto longo no db. Ao inves do char que seria melhor em caso de coisas como um endereço por exemplo
     - COLEÇÃO -> enum, set
   - ESPACIAIS -> geometry, point, polygon, multipolygon
- criando um banco de dados:
  ```
   CREATE DATABASE nome_banco;
  ```
- o que é uma QUERY em SQL?
  - É uma consulta em SQL. No qual o objetivo é a ação para buscar dados e trazê-los para a memória, a fim de executar procedimentos com eles.
- criando uma tabela com campos dentro
   ```
      use nome_banco;
      CREATE TABLE pessoas(
        nome varchar(30),
        peso float,
        altura float,
        sexo char(1),
        nacionalidade varchar(20)
      );
  ```
- apagando um banco de dados:
  ```
  drop database nome_campo;
  ```
- definindo caracteres lingua portuguesa
  ```
  default character set utf8mb4
  default collate utf8mb4_general_ci;
  ```
- Recriando a tabela com os caracteres:
  ```
  CREATE TABLE pessoas(
        nome varchar(30) NOT NULL,         (not null para campos que são obrigatoriamente digitaveis)
        nascimento date,
        peso decimal(5,2),                // 5 casas ao todo e 2 é a qntd de numeros após virgula. por ex: 102,24
        altura decimal(3,2),
        sexo enum('M','F'),               //só aceita esses valores
        nacionalidade varchar(20) DEFAULT 'BRASIL'   //se n for digitado nada o padrão será brasil
      )default charset =  utf8mb4;
  ```
- apagando tabelas:
```
drop TABLE nome_tabela;
```
- CAMPO CHAVE PRIMÁRIA ( PRIMARY KEY)
     - Esse campo NÃO se repete
     - Não existe dois valores iguais pra chave primária
     - SERVE COMO UM IDENTIFICAR
- usando a tabela pessoas como exemplo disso ficaria:
     ```
          CREATE TABLE pessoas(
		     id int NOT NULL auto_increment, //id vai ser um identificador(primary key) e o auto_increment calcula "o primeiro vai ser 1 e assim por diante"
           nome varchar(30) NOT NULL,
           nascimento date,
           peso decimal(5,2),  
           altura decimal(3,2),
           sexo enum('M','F'), 
           nacionalidade varchar(20) DEFAULT 'BRASIL',
           primary key(id)
         )default charset =  utf8mb4;
     ```
- comandos como CREATE TABLE e CREATE DATABASE são comandos DDL "Data definition Lenguage" ou seja comandos de definição
- Inserindo dados em uma tabela com INSERT INTO:
  ```
	 INSERT INTO nome_tabela
      ( nome ,nascimento, peso, altura, sexo, nacionalidade)
      VALUES
      ( 'natan' ,'ano-mes-dia', '55.5', '1.67', 'M', 'Brasil');
  ```
- Consultando TODOS os dados daquela tabela
  ```
	 SELECT * FROM nome_tabela;
  ```
- se usar DEFAULT como parametros na hora do INSERT caso tenha sido declarado na criação da tabela, se não inserir nenhum dado ele será preenchido com o DEFAULT, por exemplo na nossa tabela:
  - se nao preencher nacionalidade, será colocado como Brasil por ser o DEFAULT
-Caso adicione dados nas tabelas e a ordem dos dados seja igual a ordem da tabela tem uma forma simplificada do código anterior:
  ```
	 INSERT INTO pessoas VALUES
     	 (DEFAULT ,'nome' ,'ano-mes-dia', '85.5', '1.73', 'M', 'Brasil');
  ```
- Como inserir vários dados de uma vez SE a ordem for a mesma(maneira simplificado)
  ```
	
  ,0
  ```
- Comando INSERT TO é DML ( DATA MANIPULATION LANGUAGE)

- Adicionando um campo na tabela (DESSA FORMA SEMPRE SERÁ ADICIONADO POR ÚLTIMO):
  ```
	  alter table pessoas  // essa instrução é usada para adicionar, excluir ou modificar colunas em uma tabela existente.
     	 add column profissao varchar(10);   //adiciona um campo(mesma coisa q column) profissão dentro da tabela pessoas
  ```
- Mostrando a tabela e seus campos no MySQL Workbench:
  ```
	 describe nome_tabela;
  ```
- Removendo uma coluna:
  ```
	alter table nome_tabela
  	drop column nome_campo
  ```
- Escolhendo nova posição de uma nova coluna depois de outro campo
  ```
	 alter table nome_tabela
	 add column campo_adicionado varchar(20) after campo;
  ```
- Escolhendo nova posição de uma nova coluna em primeiro
  ```
	 alter table nome_tabela
	 add column campo_adicionado varchar(20) FIRST;
  ```

- Mofificando tipo da coluna e as CONSTRANGE (NÃO PODE MODIFICAR O NOME)
   ```
	 alter table pessoas
   	 modify column nome_campo varchar(10) not null;
   	// se deixar só not null(obrigatório que nao pode ser nulo) daria erro pq o sql ja adiciona um novo campo como null e 		causaria conflito, pra resolver isso coloca 		string vazia depois de DEFAULT, dessa forma:
   	 modify column nome_campo varchar(10) not null DEFAULT '';
   ```
- OBSERVAÇÃO
  - quando voce modifica uma coluna com modify, se nao existem registros anteriores nesta coluna (ou eles estao em branco), vc nao vai conseguir setar ela como not null, pq mesmo voce 	colocando default '0' os registros que ja existem nao serao modificados.
	agora se vc criar uma coluna do zero, com not null e setar o default '0' ela vai funcionar

- Renomeando coluna
  ```
	alter table nome_tabela
  	CHANGE column nome_campo novo_nome_campo tipo_dado();

  	alter table profissao
  	change column profissao prof varchar(20);
  ```
- RENOMEANDO A TABELA INETIRA
  ```
	alter table nome_tabela
  	rename to novo_nome_tabela
  ```
- criando tabela com mais conteúdos novos
  ```
	CREATE TABLE IF NOT EXISTS cursos(               //só vai criar a tabela se ela nao existir, isso pode ser usado tbm como IF EXISTS e tambem pode ser usado no DELETE
		nome varchar(30) NOT NULL unique,  //unique(unico) vai proibir que seja adicionado cursos com mesmo nome mas n é igual primary key, só parecidos
        	descricao text,
        	carga int unsigned, //não vou aceitar cursos com carga negativa então aqui economiza byte
   		total_aulas int,
        	ano year default '2023'
      )DEFAULT CHARSET = utf8mb4;
      
  ```
- adicionando chave primária depois de criar a tabela (PRIMARY KEY)
  ```
	alter table nome_tabela
  	add PRIMARY KEY(nome_campo);
  ```

- Apagando tabelas
  ```
	drop table nome_tabela
  
  		or

	drop table if exists nome_tabela
  ```
- Alter table e Drop table são DDL
- Registro,linha e tupla são a mesma coisa

- Manipulando Linhas com UPDATE, DELETE e TRUNCATE
  - Visualize a seguinte situação : foi digitado um nome erro em uma linha e vc quer mudar para o nome correto, o que fazer nessa situação?
    ```
	UPDATE nome_tabela SET nome_coluna = 'y' WHERE id_referencia = 'x';

    	um exemplo com valores reais:

    	UPDATE cursos SET nome = 'javascript' WHERE id_cursos = '3'
    ```
    - Note que eu estou dando update na tabela cursos, modificando o conteudo da coluna 'nome' para 'javascript' onde o id seja 3 . Isso pq estou pegando a primray Key como referencia e chave primária NUNCA se repete
    - Alterando duas colunas ao mesmo tempo:
      ```
	UPDATE cursos SET nome = 'javascript', ano = '2018'         // ano seria outra coluna na MESMA LINHA
	 WHERE id_cursos = '3'
      ```
      - Dessa maneira é só separar entre virgulas
- Caso de update sem estar usando where numa chave primária , o que já não deixaria alterar mais de 1 linha, vc pode usar o parametro LIMIT, por exemplo:
  ```
	 UPDATE cursos SET nome = 'javascript', ano = '2018', carga = '98'        
	 WHERE id_cursos = '3'
  	 LIMIT 1;
  ```
 	
- UPDATE É PERIGOSO , porque pode alterar várias linhas de uma vez sem querer
  - o proprio MySQL Workbench evita isso nas configurações: EDIT -> PREFERENCES -> SAFE UPDATES
 ```
 	 UPDATE cursos SET ano = '2090', carga = '998'        
	 WHERE ano = '2018'
```
	- Nesse caso ele estaria mudando o ano e a carga para esses valores em TODAS as linhas onde ano for igual a 2018 (ano = '2018')
 	- Pra evitar poderia usar o LIMIT pra não alterar várias linhas
  
- REMOVENDO UMA LINHA (DELETE)
  - pode deletar igual no update, usando uma chave primária e apagando uma linha ou usando um valor de campo como referencia e assim apagaria todas as linhas que possuir aquele valor(NOT SAFE DELETE), exemplos:
    ```
	delete from cursos where id_curso ='2'; 
    ```
   - Dessa maneira apagaria onde tem a primary key id_curso 2
   - Agora pra apagar várias linhas:
     ```
	delete from cursos where ano = '2018';
     ```
     - Aqui ele busca na tabela cursos na coluna ano estiver com valor de '2018' e apaga todas as linhas q tiverem ano como 2018

- Removendo TODAS as linhas de uma TABELA:
   ```
	TRUNCATE nome_tabela;
   ```
- DELETE, UPDATE e TRUNCATE SÃO comandos DML
- Diferença entre o TRUNCATE e o DROP TABLE:
  	- os dois comandos apagam mas o TRUNCATE apaga todos os dados mantendo a estrutura da tabela e o DROP TABLE apaga tudo, inclusive a estrutura da tabela.
  	  
- Gerenciando cópias de segurança MySQL e exportando DATABASE
  - Dump = cópia do banco de dados
  - Workbench = SERVER -> Export Database -> Export to self contained -> include create Schema
  - Vai criar uma pasta dumps em DOCUMENTOS com o arquivo SQL
- Importando banco de dados:
   - server -> Import Database
 
- Com servidores locais completos como XAMPP, LARAGON e outros voce pode analisar seu databse pelo phpMyADMIN e manipular dados por lá, onde vai mostrar os comandos sql e serve como aprendizado


- SELECT
  - Para ordenar uma tabela posso utilizar da seguinte maneira:
    ```
	SELECT * FROM nome_tabela
    	order by nome_coluna; 
    ```
  - Dessa maneira vai ordenar de acordo com a coluna depois do order by
  - Se utilizar o mesmo código com o DESC depois de nome_tabela, ele vai ordenar de cima pra baixo ou seja, o inverso
  - Selecionando apenas colunas que voce deseja filtrar:
    ```
	SELECT nome_coluna1, nome_coluna2, nome_coluna3 from nome_tabela
    	order by nome_coluna;   // que deseja ordenar
    ```
  - Selecionando LINHAS
    ```
	select * from cursos
	where ano  = '2016'    //seleciona linhas onde a coluna ano for 2016
	order by nome;
    ```

  - Selecionando LINHAS E COLUNAS
    ```
	select nome,descricao,carga, ano from cursos  
	where ano  = '2014'
	order by nome;

    //o codigo busca na coluna nome,descricao,carga e ano da tabela cursos onde a coluna ano tiver o valor de 2014 e retorna isso ordenado pelo nome de cima para baixo
    ```
  - Selecionando linhas e colunas usando operadores relacionais
    - <   MENOR
    - <=  MENOR IGUAL
    - MAIOR  >
    - MAIOR IGUAL >=
    - =   IGUAL
    - !=  DIFERENTE
    - AND  E  (SÓ VAI SELECIONAR SE TIVER UM E OUTRO)
    - OR   OU  ( SELECIONA SE PELO MENOS UMA CONDIÇÃO FOR VERDADEIRO)
    - NOT  (NAO)
      
      ```
	select nome,descricao, ano from cursos
	where ano  <= '2017'
	order by ano;

//O CODIGO BUSCA NA COLUNA nome, descricao e ano da tabela cursos onde o ano for menor ou igual a 2017 e ordena por ano
// assim retorna os campos nome, descricao e ano que tem como valor de ano menor ou igual a 2017
      ```
- BETWEEN
  - Selecionar os dados entre uma data e outra(uma faixa) por exemplo mas vale pra qualquer coluna
   ```
	select nome, ano from cursos
	where ano between '2012' and '2015'
	order by ano;

   // código seleciona os campos nome e ano da tabela cursos onde o valor da coluna ano estiver entre 2012 e 2015 e retornam os dados que estão dentro dessa condição
   ```
- IN
  - Selecionar campos com valores especificos, diferente de BETWEEN que engloba de um valor X até valor Y
    ```
	select nome, ano from cursos
	where ano in ('2012', '2015')
	order by ano;

    //o código seleciona as colunas nome e ano da tabela cursos onde ano tiver o valor de 2012 e 2015 apenas e retorna isso
    ```
  - AND
    ```
	SELECT nome,carga,ano from cursos
	where carga > 15 AND ano < 2018
	order by ano;

    //O CODIGO SÓ seleciona se um e outro forem verdadeiros, ou seja se uma coluna tiver carga maior que 15 e ano maior que 2018, ele não vai selecionar esse registro
    ```
  - Operador LIKE
    - a função do LIKE é ser parecido, "tipo". Pode ser usado pra encontrar valores que começam como determinada letra, por exemplo:
      ```
	SELECT * from cursos
	where nome like 'J%';

	//seleciona em todos os campos da tabela cursos onde o nome se pareça com J seguido de qualquer coisa. O % é um coringa, ele vai trocar por nenhum ou qualquer outro caractere
	// se colocar o % antes ele testaria o final, ou seja se algum nome termina com J
      ```
- WildCards
  - %
  - _

- Distiguindo Valores
  ```
	select distinct nacionalidade from pessoas;
  ```
  - distinct serve para eliminar repetições, então  no caso do código acima ele iria parar de retornar varios 'brasil' e retornaria mostrando só uma vez já que é a unica nacionalidade

- Funções de agregações
  - count() "quantos"
    ```
	select count(*) from cursos where carga > '50';  //aqui procura em todos os registros
    	select count(nome) from cursos where carga > '50';  // aqui procura especificamente na coluna nome

    // mas ambos retornam o mesmo valor nesse caso
    ```
  - Poderia contar na mão quantos registros tem na tabela cursos com carga maior que 50 mas o count() ja conta automaticamente.
- MAX()
  ```
	SELECT MAX(carga) from cursos order by carga;     //Aqui seleciona dentro da tabela cursos o maior elemento que tiver a maior carga 
	select max(total_aulas) from cursos where ano = '2016';   //Pedindo pro SQL ver qual foi o maior valor de aulas no ano de 2016
  
  ```
- MIN() Mesma coisa do MAX() só que minimo
- SUM() Serve pra somar
- AVG() Faz a média

-EXERCICIOS
```
1) Uma lista com o nome de todos os gafanhotos Mulheres.
2) Uma lista com os dados de todos aqueles que nasceram entre 1/Jan/2000 e 31/Dez/2015.
3) Uma lista com o nome de todos os homens que trabalham como programadores.
4) Uma lista com os dados de todas as mulheres que nasceram no Brasil e que têm seu nome iniciando com a letra J.
5) Uma lista com o nome e nacionalidade de todos os homens que têm Silva no nome, não nasceram no Brasil e pesam menos de 100 Kg.
6) Qual é a maior altura entre gafanhotos Homens que moram no Brasil?
7) Qual é a média de peso dos gafanhotos cadastrados?
8) Qual é o menor peso entre os gafanhotos Mulheres que nasceram fora do Brasil e entre 01/Jan/1990 e 31/Dez/2000?
9) Quantas gafanhotos Mulheres tem mais de 1.90cm de altura?

-----------RESPOSTAS-------------------------
#exec1 mostrando lista do nomes das pessoas,

SELECT nome from pessoas;   

#exec2
select * from pessoas where nascimento between '2000-01-01' and '2015-12-31';  #selecionado todos os dados na tabela pessoa onde na coluna nascimento estiver entre data 1 e data 2 solicitada

#exec3
SELECT nome from pessoas where sexo = 'M';

#exec4
select * from pessoas where nacionalidade = 'Brasil' and nome like  'm%a';

#exec5

select * from pessoas where peso > '60';
select * from pessoas where peso > '60' and sexo = 'M';

#exec6

select max(altura) from pessoas where sexo = 'F';

#exec7 - media de peso

select avg(peso) from pessoas;

#exec8

select min(peso) from pessoas where sexo = 'M' and nacionalidade = 'Brasil'; #and nascimento between  '1990-01-01' and '2000-12-31'

#exec9

select count(altura) from pessoas where sexo = 'F' and altura > '1.60';
```

- Agrupando registros
  ```
  	SELECT carga from cursos
	GROUP BY carga;     //Agrupamento. Esse código mostraria coluna carga e todos os valores agrupados por carga.

  	select total_aulas, count(*) from cursos
	group by total_aulas
	order by total_aulas;

  	//esse segundo código seleciona a coluna total de aulas, ele vai usar a função pra contabilizar isso na tablea cursos agrupando do campo total aula e ordenando pelo mesmo campo
  	// esse codigo vai retornar todos os total_aulas e do lado o count(*) que vai mostrar quantos de cada valor tem ou seja 1: total_aulas(10) o count(*) vai ser 1 por só ter uma linha com 10 em total_aulas
  ```
- Agrupando e agregando
  ```
	select carga, count(nome) from cursos
	group by carga;

  	// esse código ele vai selecionar cargas, contar o total de acordo com nome, pegando da tabela cursos e agrupando por carga
  ```

  ```
  	select carga, count(nome) from cursos
	group by carga
	HAVING count(nome) > 3;

	// esse código ele vai selecionar cargas, contar o total de acordo com nome, pegando da tabela cursos e agrupando por carga só que eu só vou MOSTRAR SOMENTE QUEM TIVER o contador ou seja o TOTAL 		maior que 3, ou seja ele mostraria a carga de 25 e o count 8 porque nessa tablea tem 8 cursos com carga de 25

	//ou
  
	select ano, count(*) from cursos
	group by ano
	having ano > '2013'
	order by count(*);

  	//aqui vai mostrar somente quando ano for maior que 2013
  ```
  - OBS: Só pode usar junto ao HAVING oq vc agrupou com group by
  - Exercicio realizado:
    - selecionar a coluna carga e mostrar o total de quantos cargas possuem o mesmo valor de carga FILTRANDO por ano acima de 2010 AGRUPANDO por carga SOMENTE quando carga for maior que a média de cargas:
    - Resolução
      
      ```
	select carga,count(*) from cursos  
	where ano > 2010
	group by carga
	HAVING carga > (select avg(carga) from cursos);      //o segundo select faz a media de cargas da tabela cursos, ou seja soma cada carga e divide pela quantidade, isso deixa d forma dinamica caso as cargas aumentem, por isso é melhor doq usar um valor fixo calculado do select sozinho


	EXERCICIOS:

	#exec1
	select nome, count(*) from pessoas
	group by nome;
	
	#exec2
	select * from pessoas;
	
	select sexo, count(*) from pessoas
	where nascimento > '1968-01-01'
	group by sexo;
		
	#exec3
	
	select nacionalidade, count(*) from pessoas 
	where nacionalidade != 'Brasil'
	group by nacionalidade;
	
	#exec4
	select altura, count(*) from pessoas
	where peso > '100'
	group by altura
	having  altura >  (select avg(altura));

      ```

- Modelo Relacional



- Adicionando chave estrangeira (Foreign key)
  ```
	alter table pessoas add column cursopreferido int;     //adicionando na tabela pessoas uma coluna cursopreferido int pq tem que ser o mesmo tipo da chave primaria e TAMANHO TB

	alter table pessoas
	add foreign key (cursopreferido)
	REFERENCES cursos(id_curso);

  	//DANDO UPDATE

  	update pessoas set cursopreferido = '3' where id = '1';
  

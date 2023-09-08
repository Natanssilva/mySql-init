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
	 INSERT INTO pessoas VALUES
          (DEFAULT ,'nome' ,'1962-11-21', '85.5', '1.73', 'M', 'Brasil'),
	  (DEFAULT ,'nome' ,'1948-03-21', '65.5', '1.59', 'f', 'Brasil'),
          (DEFAULT ,'nome' ,'1958-11-25', '75.5', '1.78', 'M', 'Brasil');
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

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

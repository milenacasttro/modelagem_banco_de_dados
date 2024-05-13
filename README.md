# Modelagem do Banco de Dados do Porjeto

&nbsp;&nbsp;&nbsp;&nbsp;A construção de um banco de dados para o site em desenvolvimento pelo grupo iniciou-se com a reflexão sobre quais informações deveriam ser armazenadas para consulta e exibição aos jogadores, tutores e outros tipos de usuários. Assim, foi estruturado um modelo relacional com foco em dois aspectos principais: informações dos usuários e questionários para perfilação e avaliação dos pares. A imagem abaixo ilustra a estrutura construída para o banco de dados relacional do projeto.

<img src="/ponderada.png" width="100%" >

&nbsp;&nbsp;&nbsp;&nbsp;Inicialmente, para lidar com o primeiro aspecto abordado, que são as informações dos usuários, criou-se uma entidade principal denominada "User", na qual são armazenadas todas as informações necessárias dos usuários que acessam o site, sejam eles estudantes, tutores, administradores ou observadores. Assim, definiu-se a seguinte tabela e seus atributos:

Entidade "User"
* "id" (Chave primária da tabela): Identificador único do usuário;
* "first_name": Primeiro nome do usuário;
* "sure_name": Sobrenome do usuário;
* "gender": Gênero do usuário;
* "email": Endereço de e-mail do usuário;
* "password": Senha de acesso definida pelo usuário.
* "phone_number": Número de telefone do usuário;
* "nationality": Nacionalidade do usuário;
* "nationality_2": Segunda nacionalidade do usuário (caso aplicável).

&nbsp;&nbsp;&nbsp;&nbsp;Com base na entidade "User", foram definidas as tabelas de estudante e tutor, uma vez que algumas informações necessárias são mais específicas para cada um desses tipos de usuário. Para isso, criou-se uma chave estrangeira que faz referência à tabela "User", resultando nas seguintes entidades:

Entidade "Tutor"
* "id" (Chave primária da tabela): Identificador único do tutor;
* "user_id" (chave estrangeira da tabela "User"): Referência ao usuário correspondente na tabela de usuários.
  
Entidade "Student"
* "id" (Chave primária da tabela): Identificador único do estudante;
* "user_id" (chave estrangeira da tabela "User"): Referência ao usuário correspondente na tabela de usuários;
* "birth_date": Data de aniversário do estudante;
* "cultural_background": Culturas com as quais o estudante tem contato e pratica;
* "university_id" (chave estrangeira da tabela "University"): Referência à universidade associada ao estudante.

&nbsp;&nbsp;&nbsp;&nbsp;Na tabela "Student" acima, é referenciado o id da universidade, que é uma chave estrangeira da tabela criada para armazenar as faculdades participantes e relacioná-las com o país no qual estão localizadas, também armazenado em outra tabela. Assim, foram criadas as seguintes tabelas e seus atributos:

Entidade "Country"
* "id" (Chave primária da tabela): Identificador único do país;
* "country_name": Nome do país;
  
Entidade "University"
* "id" (Chave primária da tabela): Identificador único da universidade;
* "country_id" (chave estrangeira da tabela "Country"): Referência ao país no qual a universidade está localizada;
* "university_name": Nome da universidade;

&nbsp;&nbsp;&nbsp;&nbsp;Além das relações mencionadas acima, a tabela de estudantes ainda estabelece um relacionamento com a tabela "Team", por meio da tabela intermediária "Student_team". Essa entidade foi criada devido à presença de mais de um estudante em um grupo. Assim, a tabela apresenta os seguintes atributos:

Entidade "Student_team"
* "id" (Chave primária da tabela): Identificador único da tabela;
* "student_id" (chave estrangeira da tabela "Student"): Referência ao estudante associado ao grupo;
* "team_id" (chave estrangeira da tabela "Team"): Referência à equipe à qual o estudante está associado.

&nbsp;&nbsp;&nbsp;&nbsp;A mesma estrutura foi desenvolvida para tutores e seus grupos, considerando que um tutor pode auxiliar mais de um grupo:

Entidade "Tutor_team"
* "id" (Chave primária da tabela): Identificador único da tabela;
* "tutor_id" (chave estrangeira da tabela "Tutor"): Referência ao tutor associado ao grupo;
* "team_id" (chave estrangeira da tabela "Team"): Referência à equipe à qual o tutor está associado.

&nbsp;&nbsp;&nbsp;&nbsp;Após estabelecer as relações intermediárias com as tabelas acima, foi possível construir a entidade que apresenta as informações dos grupos participantes:

Entidade "Team"
* "id" (Chave primária da tabela): Identificador único do grupo;
* "color": Cor do grupo;
* "universe": Universo no qual a equipe está localizada;
* "game_id" (chave estrangeira da tabela "Game"): Referência ao jogo Cesim associado ao grupo;
* "student_team_id" (chave estrangeira da tabela "Student_team"): Referência aos estudantes que compõem o grupo;
* "tutor_team_id" (chave estrangeira da tabela "Tutor_team"): Referência ao tutor responsável pelo grupo.

&nbsp;&nbsp;&nbsp;&nbsp;Na tabela acima, há uma chave estrangeira que traz o id da tabela "game", na qual são armazenadas informações sobre a edição do jogo em curso. Essa entidade apresenta os seguintes atributos:

Entidade "Game"
* "id" (chave primária da tabela): Identificador único do jogo;
* "startdate": Data de início do jogo;
* "enddate": Data de término do jogo.

&nbsp;&nbsp;&nbsp;&nbsp;Cada edição do "Cesim Global Challenge" apresenta rodadas, que são as jogadas feitas pelos grupos. Para armazenar informações sobre essas rodadas, foi criada uma tabela "Round":

Entidade "Round"
* "game_id" (chave estrangeira da tabela "Game"): Referência à edição do jogo associada a esse round;
* "round_number": Número do round;
* "startdate": Data de início do round;
* "enddate": Data de término do round.

&nbsp;&nbsp;&nbsp;&nbsp;Para lidar com o segundo aspecto abordado, que são os questionários, foram criadas tabelas para armazenar as perguntas, suas alternativas de resposta e as respostas dadas pelos jogadores. Assim, as entidades e atributos criados foram:

Entidade "Questionnaire"
* "id" (chave primária da tabela): Identificador único do questionário;
* "questionnaire_name": Nome do questionário;
* "round_id" (chave estrangeira da tabela "Round"): Referência ao round no qual o questionário é aplicado.

&nbsp;&nbsp;&nbsp;&nbsp;Considerando que um questionário pode conter várias perguntas e pensando na possibilidade de alterá-las facilmente, foi criada uma tabela associada à entidade questionário para armazenar as perguntas de cada um deles:

Entidade "Question"
* "id" (chave primária da tabela): Identificador único da pergunta;
* "question_text": Texto da pergunta;
* "questionnaire_id" (chave estrangeira da tabela "Questionnaire"): Referência ao questionário ao qual a pergunta está associada.
  
&nbsp;&nbsp;&nbsp;&nbsp;Da mesma forma, foi criada uma tabela para armazenar as alternativas de resposta:

Entidade "Alternatives"
* "id" (chave primária da tabela): Identificador único da alternativa;
* "alternative_text": Texto da alternativa;
* "question_id" (chave estrangeira da tabela "Question"): Referência à pergunta à qual a alternativa está associada.
  
&nbsp;&nbsp;&nbsp;&nbsp;Além disso, para armazenar as respostas enviadas pelos estudantes para esses questionários, foi criada uma entidade "Answers":

Entidade "Answers"
* "id" (chave primária da tabela): Identificador único da resposta do usuário;
* "student_id" (chave estrangeira da tabela "Student"): Referência ao estudante associado à resposta;
* "question_id" (chave estrangeira da tabela "Question"): Referência à pergunta à qual a resposta está associada;
* "alternative_id" (chave estrangeira da tabela "Alternative"): Referência à alternativa escolhida como resposta.

&nbsp;&nbsp;&nbsp;&nbsp;Algumas respostas dadas para os questionários podem se referir a perguntas feitas sobre os membros do grupo. Para esses casos, foi criada uma tabela que apresenta o estudante receptor das respostas:

Entidade "Answer_peer_review"
* "answer_id" (chave estrangeira da tabela "Answers"): Referência à resposta dada à pergunta;
* "receiver_user_id" (chave estrangeira da tabela "Student"): Referência ao estudante que recebe o feedback, ou seja, a resposta da pergunta.

&nbsp;&nbsp;&nbsp;&nbsp;A modelagem descrita acima foi feita por meio da plataforma "SQL Designer" e abaixo apresenta-se seu arquivo XML:
```xml
<?xml version="1.0" encoding="utf-8" ?>
<!-- SQL XML created by WWW SQL Designer, https://github.com/ondras/wwwsqldesigner/ -->
<!-- Active URL: https://sql.toad.cz/ -->
<sql>
<datatypes db="mysql">
	<group label="Numeric" color="rgb(238,238,170)">
		<type label="Integer" length="0" sql="INTEGER" quote=""/>
	 	<type label="TINYINT" length="0" sql="TINYINT" quote=""/>
	 	<type label="SMALLINT" length="0" sql="SMALLINT" quote=""/>
	 	<type label="MEDIUMINT" length="0" sql="MEDIUMINT" quote=""/>
	 	<type label="INT" length="0" sql="INT" quote=""/>
		<type label="BIGINT" length="0" sql="BIGINT" quote=""/>
		<type label="Decimal" length="1" sql="DECIMAL" re="DEC" quote=""/>
		<type label="Single precision" length="0" sql="FLOAT" quote=""/>
		<type label="Double precision" length="0" sql="DOUBLE" re="DOUBLE" quote=""/>
	</group>

	<group label="Character" color="rgb(255,200,200)">
		<type label="Char" length="1" sql="CHAR" quote="'"/>
		<type label="Varchar" length="1" sql="VARCHAR" quote="'"/>
		<type label="Text" length="0" sql="MEDIUMTEXT" re="TEXT" quote="'"/>
		<type label="Binary" length="1" sql="BINARY" quote="'"/>
		<type label="Varbinary" length="1" sql="VARBINARY" quote="'"/>
		<type label="BLOB" length="0" sql="BLOB" re="BLOB" quote="'"/>
	</group>

	<group label="Date &amp; Time" color="rgb(200,255,200)">
		<type label="Date" length="0" sql="DATE" quote="'"/>
		<type label="Time" length="0" sql="TIME" quote="'"/>
		<type label="Datetime" length="0" sql="DATETIME" quote="'"/>
		<type label="Year" length="0" sql="YEAR" quote=""/>
		<type label="Timestamp" length="0" sql="TIMESTAMP" quote="'"/>
	</group>
	
	<group label="Miscellaneous" color="rgb(200,200,255)">
		<type label="ENUM" length="1" sql="ENUM" quote=""/>
		<type label="SET" length="1" sql="SET" quote=""/>
		<type label="Bit" length="0" sql="bit" quote=""/>
	</group>
</datatypes><table x="833" y="260" name="User">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="first_name" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<row name="sure_name" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<row name="gender" null="1" autoincrement="0">
<datatype>MEDIUMTEXT</datatype>
<default>NULL</default></row>
<row name="email" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<row name="password" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<row name="phone_number" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="nationality" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<row name="nationality_2" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1013" y="525" name="Tutor">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="user_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="User" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1028" y="175" name="Student">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="user_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="User" row="id" />
</row>
<row name="birth_date" null="1" autoincrement="0">
<datatype>DATE</datatype>
<default>NULL</default></row>
<row name="cultural_background" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<row name="university_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="University" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1443" y="379" name="Team">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="color" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<row name="universe" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<row name="game_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Game" row="id" />
</row>
<row name="student_team_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Student_team" row="id" />
</row>
<row name="tutor_team_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Tutor_team" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="686" y="187" name="Country">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="country_name" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="726" y="43" name="University">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="country_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Country" row="id" />
</row>
<row name="university_name" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1562" y="8" name="Question">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="question_text" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<row name="questionnaire_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Questionnaire" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1874" y="53" name="Questionnaire">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="questionnaire_name" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="round_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Round" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1390" y="89" name="Answers">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="student_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Student" row="id" />
</row>
<row name="question_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Question" row="id" />
</row>
<row name="alternative_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Alternatives" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1138" y="19" name="Answer_peer_review">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="answer_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Answers" row="id" />
</row>
<row name="receiver_user_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Student" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1720" y="536" name="Game">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="start_date" null="1" autoincrement="0">
<datatype>TIMESTAMP</datatype>
<default>NULL</default></row>
<row name="end_date" null="1" autoincrement="0">
<datatype>TIMESTAMP</datatype>
<default>NULL</default></row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1945" y="419" name="Round">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="game_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Game" row="id" />
</row>
<row name="round_number" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="start_date" null="1" autoincrement="0">
<datatype>TIMESTAMP</datatype>
<default>NULL</default></row>
<row name="end_date" null="1" autoincrement="0">
<datatype>TIMESTAMP</datatype>
<default>NULL</default></row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1704" y="319" name="Student_team">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="student_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Student" row="id" />
</row>
<row name="team_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Team" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1174" y="409" name="Tutor_team">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="tutor_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Tutor" row="id" />
</row>
<row name="team_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Team" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
<table x="1778" y="175" name="Alternatives">
<row name="id" null="1" autoincrement="1">
<datatype>INTEGER</datatype>
<default>NULL</default></row>
<row name="alternative_text" null="1" autoincrement="0">
<datatype>VARCHAR</datatype>
<default>NULL</default></row>
<row name="question_id" null="1" autoincrement="0">
<datatype>INTEGER</datatype>
<default>NULL</default><relation table="Question" row="id" />
</row>
<key type="PRIMARY" name="">
<part>id</part>
</key>
</table>
</sql>
```
&nbsp;&nbsp;&nbsp;&nbsp;Além disso, para a construção do banco de dados relacional idealizado no sistema gerenciador de banco de dados PostgreSQL, que é gerenciado por meio do DBeaver, uma ferramenta de administração de banco de dados é necessário empregar uma série de comandos SQL. Diante disso, expõem-se abaixo os comandos necessários para a criação do banco.
```sql
DROP TABLE IF EXISTS "User";
CREATE TABLE "User" (
  "id" SERIAL PRIMARY KEY,
  "first_name" VARCHAR,
  "sure_name" VARCHAR,
  "gender" TEXT,
  "email" VARCHAR,
  "password" VARCHAR,
  "phone_number" INTEGER,
  "nationality" VARCHAR,
  "nationality_2" VARCHAR
);

DROP TABLE IF EXISTS "Tutor";
CREATE TABLE "Tutor" (
  "id" SERIAL PRIMARY KEY,
  "user_id" INTEGER REFERENCES "User" ("id")
);

DROP TABLE IF EXISTS "Student";
CREATE TABLE "Student" (
  "id" SERIAL PRIMARY KEY,
  "user_id" INTEGER REFERENCES "User" ("id"),
  "birth_date" DATE,
  "cultural_background" VARCHAR,
  "university_id" INTEGER REFERENCES "University" ("id")
);

DROP TABLE IF EXISTS "Team";
CREATE TABLE "Team" (
  "id" SERIAL PRIMARY KEY,
  "color" VARCHAR,
  "universe" VARCHAR,
  "game_id" INTEGER REFERENCES "Game" ("id"),
  "student_team_id" INTEGER REFERENCES "Student_team" ("id"),
  "tutor_team_id" INTEGER REFERENCES "Tutor_team" ("id")
);

DROP TABLE IF EXISTS "Country";
CREATE TABLE "Country" (
  "id" SERIAL PRIMARY KEY,
  "country_name" VARCHAR
);

DROP TABLE IF EXISTS "University";
CREATE TABLE "University" (
  "id" SERIAL PRIMARY KEY,
  "country_id" INTEGER REFERENCES "Country" ("id"),
  "university_name" VARCHAR
);

DROP TABLE IF EXISTS "Question";
CREATE TABLE "Question" (
  "id" SERIAL PRIMARY KEY,
  "question_text" VARCHAR,
  "questionnaire_id" INTEGER REFERENCES "Questionnaire" ("id")
);

DROP TABLE IF EXISTS "Questionnaire";
CREATE TABLE "Questionnaire" (
  "id" SERIAL PRIMARY KEY,
  "questionnaire_name" INTEGER,
  "round_id" INTEGER REFERENCES "Round" ("id")
);

DROP TABLE IF EXISTS "Answers";
CREATE TABLE "Answers" (
  "id" SERIAL PRIMARY KEY,
  "student_id" INTEGER REFERENCES "Student" ("id"),
  "question_id" INTEGER REFERENCES "Question" ("id"),
  "alternative_id" INTEGER REFERENCES "Alternatives" ("id")
);

DROP TABLE IF EXISTS "Answer_peer_review";
CREATE TABLE "Answer_peer_review" (
  "id" SERIAL PRIMARY KEY,
  "answer_id" INTEGER REFERENCES "Answers" ("id"),
  "receiver_user_id" INTEGER REFERENCES "Student" ("id")
);

DROP TABLE IF EXISTS "Game";
CREATE TABLE "Game" (
  "id" SERIAL PRIMARY KEY,
  "start_date" TIMESTAMP,
  "end_date" TIMESTAMP
);

DROP TABLE IF EXISTS "Round";
CREATE TABLE "Round" (
  "id" SERIAL PRIMARY KEY,
  "game_id" INTEGER REFERENCES "Game" ("id"),
  "round_number" INTEGER,
  "start_date" TIMESTAMP,
  "end_date" TIMESTAMP
);

DROP TABLE IF EXISTS "Student_team";
CREATE TABLE "Student_team" (
  "id" SERIAL PRIMARY KEY,
  "student_id" INTEGER REFERENCES "Student" ("id"),
  "team_id" INTEGER REFERENCES "Team" ("id")
);

DROP TABLE IF EXISTS "Tutor_team";
CREATE TABLE "Tutor_team" (
  "id" SERIAL PRIMARY KEY,
  "tutor_id" INTEGER REFERENCES "Tutor" ("id"),
  "team_id" INTEGER REFERENCES "Team" ("id")
);

DROP TABLE IF EXISTS "Alternatives";
CREATE TABLE "Alternatives" (
  "id" SERIAL PRIMARY KEY,
  "alternative_text" VARCHAR,
  "question_id" INTEGER REFERENCES "Question" ("id")
);

ALTER TABLE "Tutor" ADD FOREIGN KEY ("user_id") REFERENCES "User" ("id");
ALTER TABLE "Student" ADD FOREIGN KEY ("user_id") REFERENCES "User" ("id");
ALTER TABLE "Student" ADD FOREIGN KEY ("university_id") REFERENCES "University" ("id");
ALTER TABLE "Team" ADD FOREIGN KEY ("game_id") REFERENCES "Game" ("id");
ALTER TABLE "Team" ADD FOREIGN KEY ("student_team_id") REFERENCES "Student_team" ("id");
ALTER TABLE "Team" ADD FOREIGN KEY ("tutor_team_id") REFERENCES "Tutor_team" ("id");
ALTER TABLE "University" ADD FOREIGN KEY ("country_id") REFERENCES "Country" ("id");
ALTER TABLE "Question" ADD FOREIGN KEY ("questionnaire_id") REFERENCES "Questionnaire" ("id");
ALTER TABLE "Questionnaire" ADD FOREIGN KEY ("round_id") REFERENCES "Round" ("id");
ALTER TABLE "Answers" ADD FOREIGN KEY ("student_id") REFERENCES "Student" ("id");
ALTER TABLE "Answers" ADD FOREIGN KEY ("question_id") REFERENCES "Question" ("id");
ALTER TABLE "Answers" ADD FOREIGN KEY ("alternative_id") REFERENCES "Alternatives" ("id");
ALTER TABLE "Answer_peer_review" ADD FOREIGN KEY ("answer_id") REFERENCES "Answers" ("id");
ALTER TABLE "Answer_peer_review" ADD FOREIGN KEY ("receiver_user_id") REFERENCES "Student" ("id");
ALTER TABLE "Round" ADD FOREIGN KEY ("game_id") REFERENCES "Game" ("id");
ALTER TABLE "Student_team" ADD FOREIGN KEY ("student_id") REFERENCES "Student" ("id");
ALTER TABLE "Student_team" ADD FOREIGN KEY ("team_id") REFERENCES "Team" ("id");
ALTER TABLE "Tutor_team" ADD FOREIGN KEY ("tutor_id") REFERENCES "Tutor" ("id");
ALTER TABLE "Tutor_team" ADD FOREIGN KEY ("team_id") REFERENCES "Team" ("id");
ALTER TABLE "Alternatives" ADD FOREIGN KEY ("question_id") REFERENCES "Question" ("id");
```

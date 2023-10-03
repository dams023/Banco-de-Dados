# Banco-de-Dados

-- Comando para seleção
SELECT * FROM produtos ORDER BY id ASC;

-- DDL -Linguagem de definição de dados (Interagir com a tabela inteira).
-- Criação de tabela
CREATE TABLE produtos(
	id INT PRIMARY KEY NOT NULL,
	name VARCHAR(225),
	preco DECIMAL(10,2)
);

-- Excluir uma tabela.
DROP TABLE produtos;

-- Alterar tabela
ALTER TABLE produtos ADD descricao TEXT;

-- DML - Linguagem de manipulação de dados (Mexer com linha).

-- Inserir dado na tabela.
INSERT INTO produtos(id, nome, preco) VALUES
(1, 'Nescau Radical', 8.00),
(2, 'Yakult Activia', 4.00),
(3, 'Alpino amargo', 5.00),
(4, 'Miojo Mônica', 1.00);

--Alterar dado da tabela.
UPDATE produtos SET preco = 15.00 WHERE id=1;
UPDATE produtos SET nome = 'Toddy' WHERE nome LIKE 'Nescau Radical';

UPDATE produtos SET descricao = 'Armazenar em ambiente gelado'
WHERE id=2 or id=3;

UPDATE produtos SET descricao = 'Promoção'
WHERE preco < 4.00;

--Deletar dado de uma tabela.
DELETE FROM produtos WHERE nome LIKE 'Alpino amargo';
DELETE FROM produtos WHERE id= 1;

--Comandos DQL (Linguagem de consulta de dados).
--Selecionar todos(*) produtos, ordene ascendente
SELECT * FROM produtos ORDER BY id ASC;
--Ordenar pelo preço crescente(menor para maior).
SELECT * FROM produtos ORDER BY preco ASC;
--Ordenar pelo preco decrescente(maior para o menor)
SELECT * FROM produtos ORDER BY preco DESC;
--Selecionar Colunas
SELECT preco, nome FROM produtos ORDER BY id;
--Selecionar produtos maior que R$4,00
SELECT preco, nome FROM produtos WHERE preco > 4.00;
--Selecionar produto mais caro.
SELECT MAX (preco), AS titulo FROM produtos;
SELECT preco, nome FROM produtos ORDER BY preco DESC LIMIT 1;


--Simulando busca por nome exato.
SELECT * FROM produtos WHERE nome LIKE 'Nescau Radical';
--Busca pelos primeiros caracteres, o resto não importa.
SELECT * FROM produtos WHERE nome LIKE 'Ne%';
--A parte anterior não importa, busca o último.
SELECT * FROM produtos WHERE nome LIKE '%Fit';
--Busca por qualquer parte do nome.
SELECT * FROM produtos WHERE nome '%Nestle%';

------------------------ Fase 2 ------------------------------

----------------- Criando tabela Funcionário -----------------

CREATE TABLE funcionario(
	id SERIAL PRIMARY KEY, 
	nome VARCHAR(255) NOT NULL,
	data_nasc DATE,
	sexo CHAR(1),
	salario DECIMAL(10, 2),
	ativo BOOLEAN
);

SELECT * FROM funcionario;

--Aspas para texto ou data
INSERT INTO funcionario (nome, data_nasc, sexo, salario, ativo) VALUES
('Bob','1990-05-15', 'M', 2000.00, true),
('Augusto', '1970-04-01', 'M', 1500.00, false),
('Joanir', '2000-01-01', 'F', 1800.00,true),
('Eliza', '1995-10-02','F', 1900.00, true);

SELECT * FROM funcionario;

--Seleciona funcionários que nasceram após 01-01-1992
SELECT * FROM funcionario WHERE data_nasc > '1992-01-01';

--Seleciona funcionários em um periodo.
SELECT nome, data_nasc FROM funcionario 
WHERE data_nasc BETWEEN '1990-01-01' AND '1997-01-01';

--Contabilizar 
SELECT sexo 
COUNT (*) AS total_pessoas
AVG(salario) AS media_salarial
FROM pessoas GROUP BY sexo;

--Seleciona funcionário inativo

SELECT * FROM funcionário WHERE ativo = false;

------------------------------------------------------------------------------

--ATIVIDADE

--1. Criar uma tabela "Clientes" com campo para ID, Nome e Email. V
--2. Adicionar uma coluna "Telefone" a tabela "Clientes" usando ALTER TABLE. V
--3. Remover a coluna "Email" de tabela "Clientes" usando ALTER TABLE. V
--4. Criar uma tabela "itens" com campo para ID, Nome e Preço.
--5. Inserir um novo cliente na tabela "Clientes" (INSERT). V
--6. Inserir três novos itens na tabela "Itens" (INSERT).
--7. Atualizar o nome de uma cliente na tabela "Clientes" (UPDATE). V
--8. Atualizar o preço de um item na tabela "Itens" (UPDATE).
--9. Excluir um cliente da tabela "Clientes" (DELETE).
--10. Excluir um item da tabela "Itens" (DELETE).

-------------------------------------------------------------------------------
--PRIMEIRA TABELA

SELECT * FROM clientes ORDER BY id;

--APAGA A TABELA
DROP TABLE clientes;

CREATE TABLE clientes(
	id SERIAL PRIMARY KEY, 
	nome VARCHAR(255) NOT NULL,
	email VARCHAR(255) NOT NULL,
	sexo CHAR(1),
	ativo BOOLEAN
);


INSERT INTO clientes(nome, email, sexo) VALUES
('Nick', 'nickmayer@gmail.com', 'M'),
('Yori', 'yoriorfon@gmail.com', 'M'),
('Lucy', 'lucysmith@gmail.com', 'F');

ALTER TABLE clientes ADD telefone VARCHAR(20);

ALTER TABLE clientes DROP email;

UPDATE clientes SET nome = 'Andy' WHERE nome LIKE 'Nick';

DELETE FROM clientes WHERE nome = 'Lucy';

-------------------------------------------------------------------------------
--SEGUNDA TABELA

SELECT * FROM itens ORDER BY id;
DROP TABLE itens;

CREATE TABLE itens(
	id SERIAL PRIMARY KEY,
	nome VARCHAR(225),
	preco DECIMAL(10,2)
);

INSERT INTO itens(nome, preco) VALUES
('Nescau', 9.00),
('Yakult', 4.20),
('Alpino', 6.50);



UPDATE itens SET preco = 10.90 WHERE id=1;
DELETE FROM itens WHERE id=1;

----------------------------------------------------------------------------
DROP TABLE estados

-- Entendedo relacionamentos entre tabelas no banco de dados.

-- Criação das tabelas.
CREATE TABLE estados(
	id_estado SERIAL PRIMARY KEY,
	nome_estado VARCHAR(50) NOT NULL,
	sigla CHAR(2)
);

CREATE TABLE cidades(
	id_cidade SERIAL PRIMARY KEY,
	nome_cidade VARCHAR(50),
	cep VARCHAR(50),
	id_estado INT REFERENCES estados (id_estado) -- FK chave estrangeira.
);

--INSERINDO DADOS NAS TABELAS 
INSERT INTO estados (id_estado, nome_estado, sigla) VALUES
(1,'Paraná', 'PR'),
(2,'São Paulo', 'SP'),
(3,'Minas Gerais', 'MG');

INSERT INTO cidades (id_cidade, nome_cidade, cep, id_estado) VALUES
(10,'Nova Londrina', '87970-000',1),
(11,'Marilena', '87960-000',1),
(12,'Itaúna', '87980-000',1),
(13,'Primavera', '19273-000',2),
(14,'Rosana', '19274-000',2),
(15,'Cachoeira de prata', '35765-000',3);


SELECT * FROM cidades;
SELECT * FROM estados;

--SELECIONAR TODAS AS CIDADES E SEUS ESTADOS
SELECT cidades.nome_cidade, estados.nome_estado
FROM estados INNER JOIN cidades
ON cidades.id_estado;

DROP TABLE cidades;

--SELECIONAR TODAS AS CIDADES E SEUS ESTADOS
SELECT cidades.nome_cidades, estados.nome_estados
FROM estados INNER JOIN cidades
ON cidades.id_estado = estado.id_estado;

--SELECIONA TODAS AS CIDADES DO PARANÁ 
SELECT cidade.nome_cidade, estados.sigla
FROM estados INNER JOIN cidades
ON cidades.id_estado = estados.id_estados
WHERE estados.sigla LIKE 'SP' OR estados.sigla LIKE 'PR'
ORDER BY cidades.nome_cidade ASC;

-- SELECIONA OS ESTADOS COM MAIS CIDADES.
SELECT estados.nome_estado, COUNT(cidades.id_cidade) AS total_cidades
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
GROUP BY estados.nome_estado
ORDER BY total_cidades DESC;

-------------------------------------------------------------------------

-- CRIANDO MAIS UMA TABELA PARA O RELACIONAMENTO DE CIDADE-ESTADO.
CREATE TABLE pessoa(
	id_pessoa SERIAL PRIMARY KEY,
	nome_pessoa VARCHAR(60),
	data_nascimento DATE,
	sexo CHAR(1),
	estado_civil VARCHAR(60),
	profissao VARCHAR(60),
	id_cidade INT REFERENCES cidades(id_cidade)
);

INSERT INTO pessoa (id_pessoa, nome_pessoa, data_nascimento, sexo, estado_civil, profissao, id_cidade) VALUES
(1, 'Marcela', '2000-01-01', 'F', 'Solteira', 'Arquiteta', 10),
(2, 'Ananias', '1980-02-20', 'M', 'Casado', 'Carpinteiro', 10),
(3, 'Silvio', '1950-10-10', 'M', 'Casado', 'Dublador', 11),
(4, 'Léo', '2001-01-02', 'M', 'Casado', 'Entregador', 11),
(5, 'Chris', '1990-05-05', 'M', 'Solteiro', 'Fisiculturista', 12),
(6, 'Ana', '1997-04-01', 'F', 'Solteira', 'Cantora', 13),
(7, 'Jaime', '1970-03-01', 'M', 'Casado', 'Carteiro', 14),
(8, 'Alice', '2002-01-10', 'F', 'Solteira', 'Advogada', 15),
(9, 'Valentina', '1993-05-03', 'F', 'Solteira', 'Zootecnista', 15),
(10, 'Laura', '1998-05-09', 'F', 'Solteira', 'Balconista', 15);

SELECT * FROM pessoa;

--Seleciona estado, cidade, pessoa.
SELECT pessoa.nome_pessoa, cidade.nome_cidade
FROM pessoa INNER JOIN cidade
ON pessoa.id_cidade = cidade.id_cidade;

--Seleciona Estado,Cidade, Pessoa. 
SELECT pessoa.nome_pessoa, cidades.nome_cidade, estados.nome_estado
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
INNER JOIN estados 
ON cidades.id_estado = estados.id_estado;

--Seleciona pessoa do estado do Paraná.
SELECT pessoa.nome_pessoa, estados.nome_estado, cidades.nome_cidade
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE estados.nome_estado LIKE 'Paraná';

--Selecione o nome da pessoa, profissão e qual cidade pertence.
SELECT pessoa.nome_pessoa, cidades.nome_cidade, pessoa.profissao 
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade;

--Selecionar cidade com mais mulher/homem.
SELECT cidades.nome_cidade, COUNT(*) AS Quantidade
FROM cidades INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE pessoa.sexo = 'F'
GROUP BY cidades.id_cidade
ORDER BY Quantidade desc;

-------------------------------------------------------------------------

--1. Selecione todas as pessoas;
--2. Selecione todas as pessoas do sexo masculino "M";
--3. Selecione todas as pessoas com estado civil solteiro;
--4. Selecione pessoas e a sua profissão;
--5. Selecione as pessoas que nasceram entre 1980-01-01 e 2000-01-01;
--6. Selecione as pessoas do estado de São Paulo.

--------------------------------------------------------------------------

--1. Selecione todas as pessoas.
SELECT pessoa.nome_pessoa FROM pessoa;

--2. Selecione todas as pessoas do sexo masculino "M".
SELECT pessoa.nome_pessoa, pessoa.sexo FROM pessoa
WHERE pessoa.sexo = 'M';

--3. Selecione todas as pessoas com estado civil solteiro.
SELECT pessoa.nome_pessoa, pessoa.estado_civil FROM pessoa
WHERE pessoa.estado_civil LIKE 'Solteiro'
OR pessoa.estado_civil LIKE 'Solteira';

SELECT pessoa.nome_pessoa, pessoa.estado_civil FROM pessoa
WHERE pessoa.estado_civil LIKE 'Solteir%';

--4. Selecione pessoas e a sua profissão.
SELECT pessoa.nome, pessoa.profissao FROM pessoas;

--5. Selecione as pessoas que nasceram entre 1980-01-01 e 2000-01-01.
SELECT pessoa.nome_pessoa, pessoa.data_nascimento
FROM pessoa WHERE data_nascimento BETWEEN '1990-01-01' AND '2000-01-01';

--6. Selecione as pessoas do estado de São Paulo.
SELECT pessoa.nome_pessoa, estados.nome_estado, cidades.nome_cidade
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE estados.nome_estado LIKE 'São Paulo';


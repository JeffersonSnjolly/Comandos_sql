# Comandos Sql


## Criando as tabelas.

CREATE TABLE usuarios (
	id INT AUTO_INCREMENT PRIMARY KEY,
	nome VARCHAR(255) NOT NULL COMMENT 'Nome do usuário',
	email VARCHAR(100) NOT NULL UNIQUE COMMENT 'Email do usuário',
	endereco VARCHAR(150) NOT NULL COMMENT 'Email  do usuário',
	data_nascimento DATE NOT NULL COMMENT 'Data de nascimento'
);




CREATE TABLE destinos (
	id INT AUTO_INCREMENT PRIMARY KEY,
	nome VARCHAR(255) NOT NULL COMMENT 'Nome do destino',
	descricao VARCHAR(255) NOT NULL COMMENT 'descricão do destino'
);



CREATE TABLE reservas(
	id INT AUTO_INCREMENT PRIMARY KEY,
	id_user INT COMMENT 'ID do usurario',
	id_destino INT COMMENT 'ID do destino',
	data_reserva DATE NOT NULL COMMENT 'Data da reserva',
	status VARCHAR(15) DEFAULT 'pendente' COMMENT 'Valores aceitos pendente, confirmado, cancelado',
	FOREIGN KEY (id_user) REFERENCES usuarios(id) ON DELETE CASCADE,
	FOREIGN KEY (id_destino) REFERENCES destinos(id) ON DELETE CASCADE
);

## Comenados CRUD	

### Inserindo dados na tabela.
- INSERT INTO usuarios (nome, email, data_nascimento, endereco) VALEUS ("Jeff Santos","jeff.santos@gmail.com","1989-02-28","R. Francisco Bajarte 157");
- INSERT INTO destinos (nome, descricao) VALUES
('Paris', 'Destino famoso pela Torre Eiffel, museus icônicos e clima romântico.'),
('Tóquio', 'Uma cidade vibrante que mistura tecnologia avançada e tradições milenares.'),
('Rio de Janeiro', 'Conhecida pelo Cristo Redentor, praias lindas e paisagens naturais impressionantes.'),
('Nova York', 'A cidade que nunca dorme, repleta de cultura, arte e arranha-céus famosos.'),
('Roma', 'Capital histórica com monumentos antigos, gastronomia marcante e cultura rica.'),
('Sydney', 'Cidade litorânea famosa pela Opera House e estilo de vida ao ar livre.'),
('Cancún', 'Paraíso tropical com praias de águas cristalinas e resorts incríveis para relaxar.');

## Select

SELECT {{lista_colunas}} FROM tabela; pode usar o * para trazer todas as colunas

ou 

SELECT {{lista_colunas}} FROM tabela WHERE {{condição}};

### Operadores 
- = Igualdade.
- <> ou != Desigualdade.
- > Maior que.
- < Menor que.
- >= Maior ou igual que.
- <= Menor ou igual que.
- LIKE Comparação de padrões.
- IN Pertence a uma lista de valores.
- BETWEEN Dentre de um intervalo "Praia" procura o termo nos campos.
- AND E lógico.
- OR OU lógico.

### Ex:

- SELECT * FROM destinos;
- SELECT * FROM destinos WHERE id = 1;
- SELECT * FROM destinos WHERE id = 1 AND nome LIKE "%Prainha%";


## Update

UPDATE tabela SET coluna1 = novo_valor, coluna2 = novo_valor WHERE condição;

### Ex: 
- UPDATE destinos SET descricao = 'Um lugar maravilhoso.' WHERE id = 2; 

## Delete

DELETE FROM tabela WHERE condicao;

### Ex: 
- DELETE FROM destinos WHERE id = 4;

## DROP TABLE

DROP TABLE tabela;

### Ex: 
- DROP TABLE destinos;

## DROP coluna

### Ex: 
- ALTER TABLE nome_da_tabela DROP COLUMN nome_da_coluna;

## Alter Table

### Ex:
- ALTER TABLE novos_usuarios RENAME usuarios;
- ALTER TABLE usuarios MODIFY COLUMN endereco VARCHAR(150);

- ALTER TABLE clientes CHANGE COLUMN telefone tel_contato VARCHAR(20)


## Adicionando colunas

### EX:
- ALTER TABLE usuarios
ADD rua VARCHAR(100),
ADD numero VARCHAR(8),
ADD cidade VARCHAR(60),
ADD estado CHAR(2),
ADD pais VARCHAR(30);


### Prenchendo o campo
- UPDATE usuarios
SET rua = 'Rua das Flores', numero = '120', cidade = 'São Paulo', estado = 'SP', pais = 'Brasil'
WHERE id = 1;


## Chaves estrangeira

### Ex:

- CREATE TABLE tabela(
	id INT PRIMARY KEY AUTO_INCREMENT,
	chave_estrangeira INT,
	FOREIGN KEY (chave_estrangeira) REFERENCES outra_tabela(id) ON DELETE CASCADE
);


Consultas Avançadas com junções e sub-consultas

## Join

### Tipos 

- INNER JOIN.
- LEFT JOIN ou LEFT OUTER JOIN.
- RIGTH JOIN ou RIGTH OUTER JOIN.
- FULL JOIN ou FULL OUTER JOIN.


## INNER JOIN.

### Ex:
- SELECT * FROM tabela1 INNER JOIN tabela2 ON tabela1.coluna = tabela2.coluna;

### Com 2 Tabelas
- SELECT * FROM usuarios us INNER JOIN reservas rs ON us.id = rs.id;

### - Com 3 Tabelas

- SELECT * FROM usuarios us INNER JOIN reservas rs ON us.id = rs.id_user
INNER JOIN destinos ds ON ds.id = rs.id_destino;

## LEFT JOIN

### Ex.

- SELECT * FROM tabela1 LEFT JOIN tabela2 ON tabela1.coluna = tabela2.coluna;

- SELECT * FROM usuarios us LEFT JOIN reservas rs ON us.id = rs.id_user LEFT JOIN destinos ds ON ds.id = rs.id_destino;


## RIGHT JOIN

### Ex.

- SELECT * FROM tabela1 RIGHT JOIN tabela2 ON tabela1.coluna = tabela2.coluna;

- SELECT * FROM reservas rs RIGHT JOIN usuarios us ON rs.id_user = us.id;

## Sub Consultas

- Elas Permitem realizar consultas mais complexas permitindo que voçê use o resultado,
de uma consulta como entrada para outra consulta.

- As Subconsultas podem ser usadas em várias partes de uma consulta:
- SELECT
- FROM
- WHERE
- HAVING
- JOIN

### Ex.

- SELECT * FROM destinos
WHERE id NOT IN (SELECT id_destino FROM reservas);

- SELECT * FROM usuarios
WHERE id NOT IN (SELECT id_user FROM reservas);

- SELECT nome, (SELECT COUNT(*) FROM reservas WHERE id_user = usuarios.id) as total_reservas FROM usuario;


## Funções Agregadas.

- COUNT: Conta o número dos Registros.
- SUM: Soma os valores de uma coluna numérica.
- AVG: Calcula a média dos valores de uma coluna numérica.
- MIN: Retorna o valor mínimo de uma coluna.
- MAX: Retorna o valor máximo de uma coluna.

### Ex.

- SELECT COUNT(*) FROM usuarios;

- SELECT COUNT(*) FROM usuarios us INNER JOIN reservas rs ON us.id = rs.id_user;

- SELECT MAX(TIMESTAMPDIFF(YEAR, data_nascimento, CURRENT_DATE())) as idade FROM usuario;

- SELECT usuarios.nome, MAX(TIMESTAMPDIFF(YEAR, data_nascimento, CURRENT_DATE())) as idade FROM usuarios;

## Agrupamento de resultados(GROUP BY).

### Ex.
- SELECT COUNT(*), id_destino FROM reservas GROUP BY id_destino;

- SELECT COUNT(*), ds.nome FROM reservas rs INNER JOIN destinos ds ON rs.id_destino = ds.id GROUP BY ds.nome; 


## Ordenação de resultados(ORDER BY).

### Ex.

- SELECT COUNT(*) as qtd_destino, id_destino FROM reservas GROUP BY id_destino ORDER BY qtd_destino;

- SELECT COUNT(*) as qtd_destino, id_destino FROM reservas GROUP BY id_destino ORDER BY qtd_destino DESC;

## Indices de busca(EXPLAIN).

### Ex.

- EXPLAIN SELECT * FROM usuarios WHERE email = "jeff.Santos@gmail.com";


## Criando um index.

### Ex.

- CREATE INDEX idx_nome ON usuarios (nome);

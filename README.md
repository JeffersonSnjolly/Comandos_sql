# 📘 Guia Completo de Comandos SQL

Este README traz uma visão clara e organizada dos principais comandos SQL usados para criação de tabelas, manipulação de dados, consultas simples e avançadas. Ideal para iniciantes e estudantes que desejam praticar SQL com exemplos reais.

---

# 🏗️ **Criação de Tabelas**

```sql
CREATE TABLE usuarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL COMMENT 'Nome do usuário',
    email VARCHAR(100) NOT NULL UNIQUE COMMENT 'Email do usuário',
    endereco VARCHAR(150) NOT NULL COMMENT 'Endereço do usuário',
    data_nascimento DATE NOT NULL COMMENT 'Data de nascimento'
);

CREATE TABLE destinos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL COMMENT 'Nome do destino',
    descricao VARCHAR(255) NOT NULL COMMENT 'Descrição do destino'
);

CREATE TABLE reservas(
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_user INT COMMENT 'ID do usuário',
    id_destino INT COMMENT 'ID do destino',
    data_reserva DATE NOT NULL COMMENT 'Data da reserva',
    status VARCHAR(15) DEFAULT 'pendente' COMMENT 'Valores aceitos: pendente, confirmado, cancelado',
    FOREIGN KEY (id_user) REFERENCES usuarios(id) ON DELETE CASCADE,
    FOREIGN KEY (id_destino) REFERENCES destinos(id) ON DELETE CASCADE
);
```

---

# ✏️ **CRUD – Inserção, Consulta, Atualização e Exclusão**

## ➕ Inserindo dados

```sql
INSERT INTO usuarios (nome, email, data_nascimento, endereco)
VALUES ("Jeff Santos", "jeff.santos@gmail.com", "1989-02-28", "R. Francisco Bajarte 157");

INSERT INTO destinos (nome, descricao) VALUES
('Paris', 'Destino famoso pela Torre Eiffel, museus icônicos e clima romântico.'),
('Tóquio', 'Cidade vibrante que mistura tecnologia avançada e tradições milenares.'),
('Rio de Janeiro', 'Conhecida pelo Cristo Redentor, belas praias e paisagens naturais.'),
('Nova York', 'A cidade que nunca dorme, repleta de cultura e arranha-céus.'),
('Roma', 'Capital histórica com monumentos antigos e gastronomia marcante.'),
('Sydney', 'Famosa pela Opera House e estilo de vida ao ar livre.'),
('Cancún', 'Paraíso tropical com praias cristalinas e resorts incríveis.');
```

---

# 🔍 **SELECT – Consultas Básicas**

```sql
SELECT colunas FROM tabela;
SELECT * FROM tabela;
SELECT colunas FROM tabela WHERE condição;
```

## 🔧 Operadores

* = Igual
* <> ou != Diferente
* > Maior
* < Menor
* > = Maior ou igual
* <= Menor ou igual
* LIKE Comparação por padrão
* IN Dentro de uma lista
* BETWEEN Dentro de um intervalo
* AND Operador lógico E
* OR Operador lógico OU

## 📌 Exemplos

```sql
SELECT * FROM destinos;
SELECT * FROM destinos WHERE id = 1;
SELECT * FROM destinos WHERE id = 1 AND nome LIKE "%Praia%";
```

---

# ✏️ **UPDATE – Atualizando Dados**

```sql
UPDATE destinos
SET descricao = 'Um lugar maravilhoso.'
WHERE id = 2;
```

---

# 🗑️ **DELETE – Removendo Registros**

```sql
DELETE FROM destinos WHERE id = 4;
```

---

# ❌ **DROP TABLE / DROP COLUMN**

```sql
DROP TABLE destinos;
ALTER TABLE usuarios DROP COLUMN endereco;
```

---

# 🛠️ **ALTER TABLE – Alterando Estruturas**

```sql
ALTER TABLE novos_usuarios RENAME usuarios;
ALTER TABLE usuarios MODIFY COLUMN endereco VARCHAR(150);
ALTER TABLE clientes CHANGE COLUMN telefone tel_contato VARCHAR(20);
```

## ➕ Adicionando colunas

```sql
ALTER TABLE usuarios
ADD rua VARCHAR(100),
ADD numero VARCHAR(8),
ADD cidade VARCHAR(60),
ADD estado CHAR(2),
ADD pais VARCHAR(30);
```

---

# 🔗 **Chaves Estrangeiras**

```sql
CREATE TABLE exemplo (
    id INT PRIMARY KEY AUTO_INCREMENT,
    chave_estrangeira INT,
    FOREIGN KEY (chave_estrangeira) REFERENCES outra_tabela(id) ON DELETE CASCADE
);
```

---

# 🔄 **Consultas Avançadas – JOIN e Subconsultas**

## 🤝 INNER JOIN

```sql
SELECT * FROM usuarios us
INNER JOIN reservas rs ON us.id = rs.id_user;

SELECT * FROM usuarios us
INNER JOIN reservas rs ON us.id = rs.id_user
INNER JOIN destinos ds ON ds.id = rs.id_destino;
```

## 🡐 LEFT JOIN

```sql
SELECT * FROM usuarios us
LEFT JOIN reservas rs ON us.id = rs.id_user
LEFT JOIN destinos ds ON ds.id = rs.id_destino;
```

## 🡒 RIGHT JOIN

```sql
SELECT * FROM reservas rs RIGHT JOIN usuarios us
ON rs.id_user = us.id;
```

## 📥 Subconsultas

```sql
SELECT * FROM destinos
WHERE id NOT IN (SELECT id_destino FROM reservas);

SELECT * FROM usuarios
WHERE id NOT IN (SELECT id_user FROM reservas);

SELECT nome,
(SELECT COUNT(*) FROM reservas WHERE id_user = usuarios.id) AS total_reservas
FROM usuarios;
```

---

# 📊 **Funções Agregadas**

* COUNT
* SUM
* AVG
* MIN
* MAX

### Exemplos

```sql
SELECT COUNT(*) FROM usuarios;
SELECT COUNT(*) FROM usuarios us INNER JOIN reservas rs ON us.id = rs.id_user;
SELECT MAX(TIMESTAMPDIFF(YEAR, data_nascimento, CURRENT_DATE())) AS idade FROM usuarios;
```

---

# 📦 **GROUP BY – Agrupamentos**

```sql
SELECT COUNT(*), id_destino FROM reservas GROUP BY id_destino;

SELECT COUNT(*), ds.nome
FROM reservas rs INNER JOIN destinos ds ON rs.id_destino = ds.id
GROUP BY ds.nome;
```

---

# 📈 **ORDER BY – Ordenação**

```sql
SELECT COUNT(*) AS qtd_destino, id_destino
FROM reservas
GROUP BY id_destino
ORDER BY qtd_destino DESC;
```

---

# 🚀 **EXPLAIN – Analisando Performance**

```sql
EXPLAIN SELECT * FROM usuarios WHERE email = "jeff.santos@gmail.com";
```

---

# ⚡ Criando Índices

```sql
CREATE INDEX idx_nome ON usuarios (nome);
```

---

📌 **Pronto!** Seu README agora está organizado, legível e pronto para ser publicado no GitHub. Se quiser adicionar imagens, exemplos extra ou estrutura de projeto, posso inserir também.


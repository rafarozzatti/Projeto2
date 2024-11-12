# Projeto 2

### Tema do Projeto: Sistema de Gerenciamento de Biblioteca

#### 1. Modelo Entidade-Relacionamento (MER)

**Entidades:**
1. **Livro** - Atributos: `ID`, `título`, `ano_publicacao`, `id_editora` (FK para Editora).
2. **Autor** - Atributos: `ID`, `nome`, `nacionalidade`.
3. **Gênero** - Atributos: `ID`, `descricao`, `categoria`, `popularidade`.
4. **Usuário** - Atributos: `ID`, `nome`, `data_cadastro`, `tipo_usuario` (ex: estudante, professor).
5. **Empréstimo** - Atributos: `ID`, `data_emprestimo`, `data_devolucao`, `status`.
6. **Editora** - Atributos: `id_editora`, `nome`, `localizacao`, `ano_fundacao`.

**Relacionamentos:**
1. **Livro - Autor (N:M)**: Um livro pode ter vários autores e um autor pode ter escrito vários livros.
2. **Livro - Gênero (N:M)**: Um livro pode pertencer a vários gêneros e um gênero pode incluir vários livros.
3. **Empréstimo - Usuário (N:1)**: Um empréstimo pertence a um único usuário, mas um usuário pode fazer vários empréstimos.
4. **Empréstimo - Livro (1:N)**: Um empréstimo pode incluir vários livros, e cada livro só pode estar em um empréstimo de cada vez.
5. **Editora - Livro (1:N)**: Cada livro é publicado por uma editora, e uma editora pode publicar vários livros.

#### 2. Modelo Relacional na 3FN (Normalização)

| Tabela         | Atributos                                                                                   |
|----------------|---------------------------------------------------------------------------------------------|
| Livro          | `ID` (PK), `titulo`, `ano_publicacao`, `id_editora` (FK)                                    |
| Autor          | `ID` (PK), `nome`, `nacionalidade`                                                          |
| Gênero         | `ID` (PK), `descricao`, `categoria`, `popularidade`                                         |
| Usuário        | `ID` (PK), `nome`, `data_cadastro`, `tipo_usuario`                                          |
| Empréstimo     | `ID` (PK), `data_emprestimo`, `data_devolucao`, `status`, `id_usuario` (FK)                 |
| Editora        | `id_editora` (PK), `nome`, `localizacao`, `ano_fundacao`                                    |
| Livro_Autor    | `id_livro` (PK, FK), `id_autor` (PK, FK)                                                    |
| Livro_Gênero   | `id_livro` (PK, FK), `id_genero` (PK, FK)                                                   |
| Emprestimo_Livro | `id_emprestimo` (PK, FK), `id_livro` (FK)                                                |

#### 3. Queries para Criação das Tabelas

```sql
CREATE TABLE Editora (
    id_editora INT PRIMARY KEY,
    nome VARCHAR(100),
    localizacao VARCHAR(100),
    ano_fundacao INT
);

CREATE TABLE Livro (
    ID INT PRIMARY KEY,
    titulo VARCHAR(255),
    ano_publicacao INT,
    id_editora INT,
    FOREIGN KEY (id_editora) REFERENCES Editora(id_editora)
);

CREATE TABLE Autor (
    ID INT PRIMARY KEY,
    nome VARCHAR(255),
    nacionalidade VARCHAR(50)
);

CREATE TABLE Gênero (
    ID INT PRIMARY KEY,
    descricao VARCHAR(100),
    categoria VARCHAR(100),
    popularidade INT
);

CREATE TABLE Usuário (
    ID INT PRIMARY KEY,
    nome VARCHAR(255),
    data_cadastro DATE,
    tipo_usuario VARCHAR(50)
);

CREATE TABLE Empréstimo (
    ID INT PRIMARY KEY,
    data_emprestimo DATE,
    data_devolucao DATE,
    status VARCHAR(50),
    id_usuario INT,
    FOREIGN KEY (id_usuario) REFERENCES Usuário(ID)
);

CREATE TABLE Livro_Autor (
    id_livro INT,
    id_autor INT,
    PRIMARY KEY (id_livro, id_autor),
    FOREIGN KEY (id_livro) REFERENCES Livro(ID),
    FOREIGN KEY (id_autor) REFERENCES Autor(ID)
);

CREATE TABLE Livro_Gênero (
    id_livro INT,
    id_genero INT,
    PRIMARY KEY (id_livro, id_genero),
    FOREIGN KEY (id_livro) REFERENCES Livro(ID),
    FOREIGN KEY (id_genero) REFERENCES Gênero(ID)
);

CREATE TABLE Emprestimo_Livro (
    id_emprestimo INT,
    id_livro INT,
    PRIMARY KEY (id_emprestimo, id_livro),
    FOREIGN KEY (id_emprestimo) REFERENCES Empréstimo(ID),
    FOREIGN KEY (id_livro) REFERENCES Livro(ID)
);
```

#### 4. Código para Inserção de Dados Aleatórios

```sql
-- Inserindo dados na tabela Editora
INSERT INTO Editora (id_editora, nome, localizacao, ano_fundacao) VALUES
(1, 'Editora A', 'São Paulo', 1950),
(2, 'Editora B', 'Rio de Janeiro', 1985);

-- Inserindo dados na tabela Livro
INSERT INTO Livro (ID, titulo, ano_publicacao, id_editora) VALUES
(1, 'Dom Casmurro', 1899, 1),
(2, 'O Cortiço', 1890, 2);

-- Inserindo dados na tabela Autor
INSERT INTO Autor (ID, nome, nacionalidade) VALUES
(1, 'Machado de Assis', 'Brasileiro'),
(2, 'Aluísio Azevedo', 'Brasileiro');

-- Inserindo dados na tabela Gênero
INSERT INTO Gênero (ID, descricao, categoria, popularidade) VALUES
(1, 'Romance', 'Ficção', 85),
(2, 'Realismo', 'Ficção', 90);

-- Inserindo dados na tabela Usuário
INSERT INTO Usuário (ID, nome, data_cadastro, tipo_usuario) VALUES
(1, 'Carlos Silva', '2024-01-15', 'Estudante'),
(2, 'Ana Souza', '2024-02-20', 'Professor');

-- Inserindo dados na tabela Empréstimo
INSERT INTO Empréstimo (ID, data_emprestimo, data_devolucao, status, id_usuario) VALUES
(1, '2024-10-01', '2024-10-15', 'Devolvido', 1),
(2, '2024-11-05', NULL, 'Pendente', 2);

-- Inserindo dados na tabela Livro_Autor
INSERT INTO Livro_Autor (id_livro, id_autor) VALUES
(1, 1),
(2, 2);

-- Inserindo dados na tabela Livro_Gênero
INSERT INTO Livro_Gênero (id_livro, id_genero) VALUES
(1, 1),
(2, 2);

-- Inserindo dados na tabela Emprestimo_Livro
INSERT INTO Emprestimo_Livro (id_emprestimo, id_livro) VALUES
(1, 1),
(2, 2);
```

#### 5. Consultas SQL Interessantes

```sql
-- 1. Listar todos os livros com seus autores
SELECT Livro.titulo, Autor.nome AS autor
FROM Livro
JOIN Livro_Autor ON Livro.ID = Livro_Autor.id_livro
JOIN Autor ON Livro_Autor.id_autor = Autor.ID;

-- 2. Listar todos os empréstimos pendentes
SELECT Empréstimo.ID, Usuario.nome, Empréstimo.data_emprestimo
FROM Empréstimo
JOIN Usuário ON Empréstimo.id_usuario = Usuário.ID
WHERE Empréstimo.status = 'Pendente';

-- 3. Contar quantos livros cada usuário já emprestou
SELECT Usuario.nome, COUNT(Emprestimo_Livro.id_livro) AS total_livros
FROM Usuario
JOIN Empréstimo ON Usuario.ID = Empréstimo.id_usuario
JOIN Emprestimo_Livro ON Empréstimo.ID = Emprestimo_Livro.id_emprestimo
GROUP BY Usuario.nome;

-- 4. Listar livros por gênero
SELECT Gênero.descricao, Livro.titulo
FROM Gênero
JOIN Livro_Gênero ON Gênero.ID = Livro_Gênero.id_genero
JOIN Livro ON Livro_Gênero.id_livro = Livro.ID;

-- 5. Encontrar o usuário com mais empréstimos
SELECT Usuario.nome, COUNT(Empréstimo.ID) AS total_emprestimos
FROM Usuario
JOIN Empréstimo ON Usuario.ID = Empréstimo.id_usuario
GROUP BY Usuario.nome
ORDER BY total_emprestimos DESC
LIMIT 1;
```

---
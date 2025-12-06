# 🎓 Sistema Escolar – Modelo Conceitual (MER/DER)

Este projeto representa um **Sistema Escolar** desenvolvido para organizar e gerenciar informações de alunos, professores, cursos, turmas e disciplinas. O banco será modelado seguindo as regras do **Modelo Entidade-Relacionamento (MER)** e implementado no **Supabase**.

---

## 📌 1. Cenário

A escola fictícia **LearnHub** (nome pode ser modificado) é uma instituição de ensino fundamental e médio que atende mais de 1.200 alunos distribuídos em 30 turmas – além de professores, coordenadores e funcionários administrativos.

Atualmente a escola enfrenta dificuldades com:
- Registros manuais e informações duplicadas  
- Falta de controle sobre matrículas e notas  
- Dificuldade em identificar quais professores lecionam cada disciplina  
- Atrasos na geração de relatórios acadêmicos  

Para resolver esses problemas, será desenvolvido um **Sistema Escolar completo**, com banco de dados relacional, que organizará:

✔ Dados dos alunos  
✔ Disciplinas e cursos  
✔ Professores e turmas  
✔ Matrículas dos alunos por disciplina

---

## 📌 2. Objetivo do Sistema

O sistema permitirá:

- Registrar e gerenciar alunos, professores, disciplinas e turmas  
- Controlar matrículas dos alunos  
- Registrar notas e frequência  
- Gerar relatórios acadêmicos confiáveis  
- Reduzir erros e automatizar processos internos  

---

## 📌 3. Entidades do Sistema

O sistema contém **7 entidades principais**, incluindo uma entidade associativa.

### **1) Aluno**
- id_aluno (PK)  
- nome  
- endereco (composto: rua, numero, bairro, cidade)  
- telefones (multivalorado)  
- data_nascimento  
- idade (derivado)

### **2) Professor**
- id_professor (PK)  
- nome  
- email  
- telefones (multivalorado)  
- especialidades (multivalorado)

### **3) Curso**
- id_curso (PK)  
- nome  
- descricao  
- duracao (anos)

### **4) Disciplina**
- id_disciplina (PK)  
- nome  
- descricao  
- carga_horaria  

### **5) Turma**
- id_turma (PK)  
- semestre  
- ano  
- horario (composto: dia_da_semana, hora_inicio, hora_fim)  
- sala  

### **6) Carteirinha** (Relacionamento 1:1 com Aluno)
- id_carteirinha (PK)  
- numero  
- data_emissao  
- validade (derivado)

### **7) Matrícula** (Entidade Associativa – N:N entre Aluno e Turma)
- id_matricula (PK)  
- data_matricula  
- status (ativa, trancada, concluída)

---

## 📌 4. Tipos de Atributos Exigidos no MER

| Tipo de atributo | Onde aparece |
|------------------|--------------|
| **Simples** | nome, email, descricao … |
| **Composto** | endereço do aluno |
| **Multivalorado** | telefones, especialidades |
| **Derivado** | idade |
| **Chave primária** | todas as entidades possuem PK |

---

## 📌 5. Relacionamentos e Cardinalidades

### **✔ (1:1) Aluno — Carteirinha**
Um aluno possui uma carteirinha e vice-versa.

### **✔ (1:N) Professor — Disciplina**
Um professor ministra várias disciplinas.

### **✔ (1:N) Curso — Turma**
Um curso possui várias turmas.

### **✔ (N:N) Aluno — Disciplina**
Representado pela entidade **Matricula**.

---

## 📌 6. DER (Diagrama Entidade-Relacionamento)

> O diagrama do projeto pode ser encontrado na pasta /docs ou abaixo na documentação visual.

## Modelo Conceitual (DER)
[LearnHub.pdf](https://github.com/user-attachments/files/23991036/LearnHub.pdf)

---

## Modelo Logico
[ModeloLogico.pdf](https://github.com/user-attachments/files/23991037/ModeloLogico.pdf)

---

# Banco de Dados (Supabase)

Este banco de dados contém as tabelas principais de um sistema acadêmico, incluindo **Alunos, Carteirinhas, Cursos, Professores, Disciplinas, Turmas e Matrículas**.  
O modelo está pronto para rodar no Supabase (PostgreSQL).

---

## Tabela Aluno
Armazena os dados pessoais dos alunos.

```sql
CREATE TABLE Aluno (
    id_aluno SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    data_nascimento DATE,
    email VARCHAR(100),
    telefone VARCHAR(20),
    rua VARCHAR(100),
    numero VARCHAR(10),
    bairro VARCHAR(50),
    cidade VARCHAR(50)
);
```
## Tabela Carteirinha
Cada aluno possui uma única carteirinha.

```sql
CREATE TABLE Carteirinha (
    id_carteirinha SERIAL PRIMARY KEY,
    numero VARCHAR(50),
    data_emissao DATE,
    validade DATE,
    id_aluno INT UNIQUE REFERENCES Aluno(id_aluno) ON DELETE CASCADE
);
```
## Tabela Curso
Armazena os cursos disponíveis na instituição.

```sql
CREATE TABLE Curso (
    id_curso SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT,
    duracao INT,
    carga_horaria INT
);
```
## Tabela Professor
Contém informações dos professores da instituição.

```sql
CREATE TABLE Professor (
    id_professor SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    telefones VARCHAR(50),
    especialidades VARCHAR(100)
);
```
## Tabela Disciplina
Cada disciplina pertence a um curso e pode ser ministrada por um professor.

```sql
CREATE TABLE Disciplina (
    id_disciplina SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT,
    id_curso INT REFERENCES Curso(id_curso) ON DELETE SET NULL,
    id_professor INT REFERENCES Professor(id_professor) ON DELETE SET NULL
);
```
## Tabela Turma
Armazena informações sobre turmas, incluindo semestre, sala, horário e ano.

```sql
CREATE TABLE Turma (
    id_turma SERIAL PRIMARY KEY,
    semestre_sala VARCHAR(50),
    horario VARCHAR(50),
    ano INT
);
```
## Tabela Matricula
Registra as matrículas dos alunos em cursos e turmas específicas.

```sql
CREATE TABLE Matricula (
    id_matricula SERIAL PRIMARY KEY,
    data_matricula DATE,
    status VARCHAR(20),
    id_aluno INT REFERENCES Aluno(id_aluno) ON DELETE CASCADE,
    id_curso INT REFERENCES Curso(id_curso) ON DELETE CASCADE,
    id_turma INT REFERENCES Turma(id_turma) ON DELETE CASCADE
);
```

---

## Inserts utilizados para popular as tabelas com informações aleatorias
inserts criados com auxilio de inteligencia artificial

## Alunos
```sql
WITH nomes AS (
    SELECT unnest(ARRAY[
        'Ana','Bruno','Carla','Daniel','Eduardo','Fernanda','Gustavo','Helena','Igor','Julia',
        'Lucas','Mariana','Natalia','Otavio','Paula','Rafael','Sofia','Tiago','Vanessa','Victor'
    ]) AS nome
),
sobrenomes AS (
    SELECT unnest(ARRAY[
        'Silva','Souza','Oliveira','Santos','Lima','Costa','Pereira','Almeida','Rocha','Carvalho',
        'Fernandes','Gomes','Martins','Barbosa','Ribeiro','Mendes','Cardoso','Azevedo','Cavalcanti','Teixeira'
    ]) AS sobrenome
)
INSERT INTO Aluno (nome, data_nascimento, email, telefone, rua, numero, bairro, cidade)
SELECT 
    n.nome || ' ' || s.sobrenome AS nome,
    date '2010-01-01' + (random()*4000)::int AS data_nascimento,
    lower(n.nome || '.' || s.sobrenome || gs || '@email.com') AS email,
    '+55' || (1000000000 + (random()*899999999)::int) AS telefone,
    'Rua ' || gs AS rua,
    (1 + (random()*1000)::int)::text AS numero,
    'Bairro ' || (1 + (random()*50)::int) AS bairro,
    'Cidade ' || (1 + (random()*50)::int) AS cidade
FROM generate_series(1,25) gs
CROSS JOIN nomes n
CROSS JOIN sobrenomes s
LIMIT 500;
```
## Carteirinhas
```sql
INSERT INTO Carteirinha (numero, data_emissao, validade, id_aluno)
SELECT 
    'CART-' || gs AS numero,
    now() - (random()*1000)::int * interval '1 day' AS data_emissao,
    now() + (365 + random()*365)::int * interval '1 day' AS validade,
    gs AS id_aluno
FROM generate_series(1,500) gs;
```
## Cursos
```sql
WITH cursos AS (
    SELECT unnest(ARRAY[
        '1º Ano Fundamental','2º Ano Fundamental','3º Ano Fundamental','4º Ano Fundamental','5º Ano Fundamental',
        '6º Ano Fundamental','7º Ano Fundamental','8º Ano Fundamental','9º Ano Fundamental',
        '1º Ano Médio','2º Ano Médio','3º Ano Médio'
    ]) AS nome
)
INSERT INTO Curso (nome, descricao, duracao, carga_horaria)
SELECT 
    c.nome || ' ' || gs AS nome,
    'Série ' || c.nome || ' da Escola Básica' AS descricao,
    1 AS duracao,
    800 + (random()*200)::int AS carga_horaria
FROM cursos c
CROSS JOIN generate_series(1,50) gs
LIMIT 500;
```
## Professores
```sql
WITH prof_nomes AS (
    SELECT unnest(ARRAY[
        'Carlos','Marcos','Paulo','Roberta','Tatiana','Renato','Flavia','Eduardo','Simone','Marcelo'
    ]) AS nome
),
prof_sobrenomes AS (
    SELECT unnest(ARRAY[
        'Santos','Almeida','Costa','Fernandes','Ribeiro','Barbosa','Mendes','Cardoso','Rocha','Lima'
    ]) AS sobrenome
),
disciplinas AS (
    SELECT unnest(ARRAY['Matemática','Português','Ciências','História','Geografia','Educação Física','Arte','Inglês']) AS disciplina
)
INSERT INTO Professor (nome, email, telefones, especialidades)
SELECT 
    n.nome || ' ' || s.sobrenome AS nome,
    lower(n.nome || '.' || s.sobrenome || gs || '@email.com') AS email,
    '+55' || (1000000000 + (random()*899999999)::int) AS telefones,
    d.disciplina AS especialidades
FROM generate_series(1,25) gs
CROSS JOIN prof_nomes n
CROSS JOIN prof_sobrenomes s
CROSS JOIN LATERAL (
    SELECT disciplina 
    FROM disciplinas 
    ORDER BY random() 
    LIMIT 1
) d
LIMIT 500;
```
## Disciplinas
```sql
WITH disciplinas AS (
    SELECT unnest(ARRAY[
        'Matemática','Português','Ciências','História','Geografia','Educação Física','Arte','Inglês'
    ]) AS nome
)
INSERT INTO Disciplina (nome, descricao, id_curso, id_professor)
SELECT 
    d.nome || ' ' || gs AS nome,
    'Disciplina de ' || d.nome || ' para alunos do ensino básico' AS descricao,
    (1 + (random()*499)::int) AS id_curso,
    (1 + (random()*499)::int) AS id_professor
FROM disciplinas d
CROSS JOIN generate_series(1,75) gs
LIMIT 500;
```
## Turmas
```sql
INSERT INTO Turma (semestre_sala, horario, ano)
SELECT 
    'Sala ' || (1 + (random()*50)::int) AS semestre_sala,
    (7 + (random()*5)::int) || ':00 - ' || (8 + (random()*5)::int) || ':50' AS horario,
    2020 + (random()*5)::int AS ano
FROM generate_series(1,500) gs;
```
## Matricula
```sql
INSERT INTO Matricula (data_matricula, status, id_aluno, id_curso, id_turma)
SELECT 
    now() - (random()*1000)::int * interval '1 day' AS data_matricula,
    CASE WHEN random() < 0.9 THEN 'Ativo' ELSE 'Inativo' END AS status,
    (1 + (random()*499)::int) AS id_aluno,
    (1 + (random()*499)::int) AS id_curso,
    (1 + (random()*499)::int) AS id_turma
FROM generate_series(1,500) gs;
```
---

## Exemplo funcional de um CRUD na tabela aluno
O código é apenas uma **demonstração** pois depois que eu executei o DELETE o SELECT e o UPDATE não vão mais funcionar pois não existe mais id 501 na tabela de alunos.

Caso queira testar, **utilize o INSERT novamente e use o novo id gerado automaticamente**

## INSERT (CREATE)
```sql
INSERT INTO aluno (nome, data_nascimento, email, telefone, rua, numero, bairro, cidade)
VALUES ('Vinicius Oliveira', '2006-05-10', 'vinicius@gmail.com', '16996655444', 'Maria Jose Abel Freitas', 614, 'Garden', 'Franca')
```
## SELECT (READ)
```sql
SELECT *
FROM aluno
WHERE id_aluno = 501 AND nome LIKE 'Vinicius%'
```
## UPDATE
```sql
UPDATE aluno
SET nome = 'Vinicius Souza'
WHERE id_aluno = 501 AND email = 'vinicius@gmail.com'
```
## DELETE
```sql
DELETE FROM aluno
WHERE id_aluno = 501
```
---
# Consultas SQL 

A seguir estão **10 consultas SQL** acompanhadas de suas respectivas **perguntas**, todas utilizando:  
**SELECT, WHERE, ORDER BY e JOIN**.

---

## 1.Listar todos os alunos com suas carteirinhas  
**Pergunta:** *Quais alunos já possuem carteirinha emitida?*

```sql
SELECT 
    a.id_aluno,
    a.nome AS aluno,
    c.numero AS numero_carteirinha,
    c.data_emissao
FROM Aluno a
JOIN Carteirinha c ON c.id_aluno = a.id_aluno
ORDER BY a.nome;
```
## 2.Listar professores e as disciplinas que eles ministram
**Pergunta:** *Quais disciplinas cada professor está responsável por ensinar?*

```sql
SELECT 
    p.nome AS professor,
    d.nome AS disciplina,
    d.descricao
FROM Professor p
JOIN Disciplina d ON d.id_professor = p.id_professor
ORDER BY p.nome;
```
## 3.Exibir todas as matrículas com nome do aluno e nome do curso
**Pergunta:** *Quais alunos estão matriculados em quais cursos?*

```sql
SELECT 
    m.id_matricula,
    a.nome AS aluno,
    c.nome AS curso,
    m.data_matricula,
    m.status
FROM Matricula m
JOIN Aluno a ON a.id_aluno = m.id_aluno
JOIN Curso c ON c.id_curso = m.id_curso
ORDER BY a.nome;
```
## 4.Ver alunos matriculados em um curso específico
**Pergunta:** *Quem está matriculado no curso "1º Ano Fundamental"?*

```sql
SELECT 
    a.nome AS aluno,
    c.nome AS curso
FROM Matricula m
JOIN Aluno a ON a.id_aluno = m.id_aluno
JOIN Curso c ON c.id_curso = m.id_curso
WHERE c.nome LIKE '1º Ano Fundamental%'
ORDER BY a.nome;
```
## 5.Listar disciplinas de um curso específico
**Pergunta:** *Quais disciplinas pertencem ao curso X?*

```sql
SELECT 
    d.nome AS disciplina,
    c.nome AS curso
FROM Disciplina d
JOIN Curso c ON d.id_curso = c.id_curso
WHERE c.nome like '1º Ano Fundamental %'
ORDER BY d.nome;
```
## 6.Listar todas as turmas e seus horários
**Pergunta:** *Quais são as turmas por ano e horário?*

```sql
SELECT 
    id_turma,
    semestre_sala,
    horario,
    ano
FROM Turma
ORDER BY ano DESC, semestre_sala;
```
## 7.Ver quais alunos estão em uma turma específica
**Pergunta:** *Quem está na turma ID = 3?*

```sql
SELECT 
    a.nome AS aluno,
    t.semestre_sala,
    t.horario,
    t.ano
FROM Matricula m
JOIN Aluno a ON a.id_aluno = m.id_aluno
JOIN Turma t ON t.id_turma = m.id_turma
WHERE t.id_turma = 3
ORDER BY a.nome;
```
## 8.Ver todos os cursos e seus professores responsáveis
**Pergunta:** *Quais professores estão atribuídos às disciplinas de cada curso?*

```sql
SELECT 
    c.nome AS curso,
    d.nome AS disciplina,
    p.nome AS professor
FROM Curso c
JOIN Disciplina d ON d.id_curso = c.id_curso
LEFT JOIN Professor p ON p.id_professor = d.id_professor
ORDER BY c.nome, d.nome;
```
## 9.Listar alunos sem carteirinha
**Pergunta:** *Quem ainda não possui carteirinha emitida?*

```sql
SELECT 
    a.id_aluno,
    a.nome
FROM Aluno a
LEFT JOIN Carteirinha c ON c.id_aluno = a.id_aluno
WHERE c.id_carteirinha IS NULL
ORDER BY a.nome;
```
## 10.Consultar cursos com maior carga horária
**Pergunta:** *Quais são os cursos mais longos?*

```sql
SELECT 
    id_curso,
    nome,
    carga_horaria
FROM Curso
ORDER BY carga_horaria DESC;
```



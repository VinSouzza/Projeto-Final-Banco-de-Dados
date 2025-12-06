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
✔ Histórico de notas e frequência  

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




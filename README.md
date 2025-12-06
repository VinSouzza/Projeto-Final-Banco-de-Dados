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

---



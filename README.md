# 🏥 Sistema de Gerenciamento de Consultas Médicas (SQL)

Este projeto simula um sistema básico de agendamento de consultas médicas utilizando **SQL Server**. Ele é ideal para fins didáticos e demonstra o uso de criação de tabelas, relacionamentos, inserção de dados e consultas analíticas com `JOIN`, `GROUP BY`, `ORDER BY` e agregações.

---

## 📂 Estrutura do Projeto

O código SQL cria e popula três tabelas principais:

### 🔸 Pacientes
Armazena informações dos pacientes:
- `id` (chave primária, auto incrementável)
- `nome`
- `data_nascimento`
- `telefone`

### 🔸 Médicos
Contém dados dos profissionais de saúde:
- `id` (chave primária, auto incrementável)
- `nome`
- `especialidade`

### 🔸 Consultas
Relaciona pacientes e médicos em datas específicas:
- `id` (chave primária, auto incrementável)
- `id_paciente` (chave estrangeira)
- `id_medico` (chave estrangeira)
- `data_consulta`
- `status` (valores permitidos: `Agendada`, `Realizada`, `Cancelada`)

---

## 🗃️ Inserção de Dados

O script insere dados fictícios para simular um ambiente real de consultas, incluindo:
- 10 pacientes
- 4 médicos
- 10 consultas com diferentes datas e status

---

## 📊 Relatórios Gerados

O código inclui consultas SQL para gerar relatórios úteis:


✅ -- Mostra quantas consultas realizadas cada médico teve:

```sql
SELECT m.nome, COUNT(c.id) AS total_consultas 
FROM Medicos m
LEFT JOIN Consultas c ON c.id_medico = m.id AND c.status = 'Realizada'
GROUP BY m.nome
ORDER BY total_consultas DESC;
```


✅ -- Lista os pacientes que mais agendam consultas:

```sql
SELECT p.nome, COUNT(c.id) AS total_consultas 
FROM Pacientes p
LEFT JOIN Consultas c ON c.id_paciente = p.id 
GROUP BY p.nome 
ORDER BY total_consultas DESC;
```


✅ -- Média de consultas por médico por mês: 

```sql
SELECT m.nome, FORMAT(c.data_consulta, 'yyyy-MM')
	AS mes, COUNT(c.id) AS total_consultas 
FROM Consultas c 
JOIN Medicos m ON c.id_medico = m.id 
WHERE c.status = 'Realizada' 
GROUP BY m.nome, FORMAT(c.data_consulta, 'yyyy-MM')
ORDER BY m.nome, FORMAT(c.data_consulta, 'yyyy-MM');
```

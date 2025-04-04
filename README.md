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
  
---

⏩ Criação da tabela Pacientes
```sql
CREATE TABLE Pacientes ( 
    id INT IDENTITY(1,1) PRIMARY KEY, 
    nome NVARCHAR(100) NOT NULL, 
    data_nascimento DATE NOT NULL, 
    telefone NVARCHAR(15) NULL 
); 
```

⏺️ Dados da tabela 
```sql
INSERT INTO Pacientes( nome, data_nascimento, telefone)
VALUES ('Patricia', '1999-05-13','11946551487'), ('Sofia', '1986-01-05', '11923654879'),
('Filomena', '1998-10-10', '11914574515'), ('Sabrina', '1998-09-04','11923654187'),
('Joana Silva','1997-02-06', '11946568451'),('Suzana Rodrigues','1996-07-22','11946512369'),
('Luana Lima', '1996-02-15','11962356215'),('Joice Maria','1994-12-10','11984576954'),
('Lucas Rocha','1999-10-06','11965432654'),('Diogo Santos','1997-05-06','11978546225');
 ```
---

### 🔸 Médicos
Contém dados dos profissionais de saúde:
- `id` (chave primária, auto incrementável)
- `nome`
- `especialidade`

  ---
  
  ⏩ Criação da tabela Medicos
  ```sql
  CREATE TABLE Medicos ( 
    id INT IDENTITY(1,1) PRIMARY KEY, 
    nome NVARCHAR(100) NOT NULL, 
    especialidade NVARCHAR(100) NOT NULL
  ); 
  ```

⏺️ Dados da tabela
  ```sql
   INSERT INTO Medicos( nome, especialidade)
 VALUES ('Dr Junior','Psicologia'), ('Dra Alessandra','Fisioterapia'),
 ('Dra Lucia','Neurologia'), ('Dra Carla','Psiquiatra');
  ```
---

### 🔸 Consultas
Relaciona pacientes e médicos em datas específicas:
- `id` (chave primária, auto incrementável)
- `id_paciente` (chave estrangeira)
- `id_medico` (chave estrangeira)
- `data_consulta`
- `status` (valores permitidos: `Agendada`, `Realizada`, `Cancelada`)
  
---

⏩ Crição da tabela Consultas
```sql
   
CREATE TABLE Consultas ( 
    id INT IDENTITY(1,1) PRIMARY KEY, 
    id_paciente INT NOT NULL, 
    id_medico INT NOT NULL, 
    data_consulta DATE NOT NULL, 
    [status] NVARCHAR(20) NOT NULL DEFAULT 'Agendada'
		CHECK ([status] IN ('Agendada', 'Realizada', 'Cancelada')), 
    FOREIGN KEY (id_paciente) REFERENCES Pacientes(id), 
    FOREIGN KEY (id_medico) REFERENCES Medicos(id) 
);

```

⏺️ Dados da tabela
```sql
 INSERT INTO Consultas (id_paciente, id_medico, data_consulta, [status])
VALUES 
    (1, 1, '2024-03-01', 'Realizada'), 
    (2, 2, '2024-03-02', 'Realizada'), 
    (3, 3, '2024-03-05', 'Agendada'), 
    (4, 4, '2024-03-07', 'Cancelada'), 
    (5, 1, '2024-03-10', 'Realizada'), 
    (6, 2, '2024-03-12', 'Agendada'), 
    (7, 3, '2024-03-15', 'Realizada'), 
    (8, 4, '2024-03-20', 'Cancelada'), 
    (9, 1, '2024-03-22', 'Agendada'), 
    (10, 2, '2024-03-25','Realizada');
```


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

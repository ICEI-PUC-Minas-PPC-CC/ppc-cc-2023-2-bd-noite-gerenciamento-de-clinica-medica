# 1. Introdução

O presente projeto tem o objetivo de modelar um sistema de armazenamento de Banco de Dados para o gerenciamento de uma clínica médica, sendo que o escopo do projeto deve ser capaz de relacionar as seguinte entidades: médicos, pacientes, remédios e prescrições. 

#### 1.1 Entidades Encontradas
Na primeira análise, quais foram as entidades encontradas?
- Médicos
- Pacientes
- Remédios
- Prescrições

#### 1.2 Contexto das Entidades
Na primeira análise, objetivos das entidades.
As quatro entidades iniciais identificadas serão registradas com o objetivo de armazenar os remédios e prescrições receitadas por seus respectivos médicos no registro dos pacientes, criando uma relação interdependente de quatro vias. 

#### 1.3  Levantamento Inicial Entidades x Atributos
Na primeira análise, quais foram os atributos iniciais encontrados?
- Médicos: ID, CRM, CPF, nome completo, data de nascimento, telefone, e-mail, especialidade
- Pacientes: ID, CPF, nome completo, data de nascimento, endereço, telefone, e-mail
- Remédios: ID, nome, laboratório, quantidade, validade, tarja, restrição, preço
- Prescrições: ID, paciente, médico, data, descrição


# 2. Relacionamentos

  - Médicos (1,n) → Pacientes (1,n): Médicos TÊM pacientes e pacientes TÊM médicos. Cada médico será vinculado aos seus pacientes e cada paciente será vinculado a pelo menos um médico.
  - Remédios (1,n) → Prescrições (1,n): Remédios TÊM prescrições e prescrições TÊM remédios. Cada remédio deverá necessariamente estar contido em uma prescrição para ser vinculado a um paciente, e cada prescrição deverá necessariamente conter pelo menos um remédio para poder ser indicada a um paciente.
  - Prescrições (1,n) → Pacientes (1,1): Prescrições SÃO RECEITADAS a um pacientes e pacientes RECEBEM prescrições. Cada prescrição deve ser vinculada a um paciente específico, enquanto que pacientes podem ser receitados com mais de uma prescrição.
  - Prescrições (1,n) → Médicos (1,1): Prescrições SÃO ESCRITAS por um médico e médicos ESCREVEM mais de uma prescrição. Cada prescrição deve ser escrita por um médico e cada médico pode escrever quantas prescrições forem necessárias. 

# 3. Modelo Conceitual

![image](https://github.com/ICEI-PUC-Minas-PPC-CC/ppc-cc-2023-2-bd-noite-gerenciamento-de-clinica-medica/assets/117238473/fc37dc31-e10e-4b23-b3d6-2d86ec73c88f)


# 4. Representação Tabular

- Médicos = (<ins>CRM</ins>, CPF, nome completo, data de nascimento, telefone, e-mail, especialidade)
- Pacientes = (<ins>CPF</ins>, nome completo, data de nascimento, endereço, telefone, e-mail)
- Remédios = (<ins>ID</ins>, nome, laboratório, quantidade, validade, tarja, restrição, preço)
- Prescrições = (<ins>ID</ins>, paciente, médico, data, descrição)

# 5. Modelo Lógico

![image](https://github.com/ICEI-PUC-Minas-PPC-CC/ppc-cc-2023-2-bd-noite-gerenciamento-de-clinica-medica/assets/117238473/f3cc159f-8a8d-41e7-adf5-796d429cb646)

# 6. Modelo Físico
```sql
CREATE DATABASE bancodados;

CREATE TABLE Medico(
    medico_id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    medico_cpf CHAR(14) NOT NULL,
    medico_crm VARCHAR(12) NOT NULL,
    medico_nome VARCHAR(200) NOT NULL,
    medico_data_nascimento DATE NOT NULL,
    medico_telefone VARCHAR(20) NOT NULL,
    medico_email VARCHAR(200) NOT NULL,
    medico_especialidade VARCHAR(100) NOT NULL
);

CREATE TABLE Paciente(
    paciente_id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    paciente_cpf CHAR(14) NOT NULL UNIQUE,
    paciente_nome VARCHAR(200) NOT NULL,
    paciente_data_nascimento DATE NOT NULL,
    paciente_telefone VARCHAR(20) NOT NULL,
    paciente_email VARCHAR(200) NOT NULL,
    paciente_endereco VARCHAR(200) NOT NULL,
);

CREATE TABLE Remedio(
    remedio_id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    remedio_nome VARCHAR(200) NOT NULL,
    remedio_laboratorio VARCHAR(200) NOT NULL,
    remedio_validade DATE NOT NULL,
    remedio_tarja VARCHAR(15) NOT NULL,
    remedio_contraindicacao VARCHAR(200) NOT NULL,
    remedio_preco FLOAT NOT NULL
);

CREATE TABLE Prescricao(
    prescricao_id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    fk_medico_id INT, 
    CONSTRAINT fk_medico_id FOREIGN KEY (fk_medico_id) REFERENCES Medico(medico_id),
    fk_paciente_id INT, 
    CONSTRAINT fk_paciente_id FOREIGN KEY (fk_paciente_id) REFERENCES Paciente(paciente_id),
    fk_remedio_id INT, 
    CONSTRAINT fk_remedio_id FOREIGN KEY (fk_remedio_id) REFERENCES Remedio(remedio_id),
 	prescricao_data DATE NOT NULL,
    prescricao_descricao VARCHAR(1000) NOT NULL
);
```


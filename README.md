# 🧠 AWS Backend-Front - Cadastro de Alunos

Este projeto foi desenvolvido como parte do **curso de DevOps oferecido pela parceria entre Santander e Alura**, no **laboratório proposto pela professora Olivia Braga**.  
O objetivo é integrar **frontend e backend** em uma aplicação completa hospedada na **AWS**, utilizando serviços como **EC2** e **RDS (MySQL)**.

---

## 🚀 Objetivo do Projeto

Criar uma aplicação web simples para **cadastro de alunos**, composta por:

- **Frontend:** página HTML/CSS/JS para entrada de dados.

- **Backend:** API em Flask (Python) que realiza operações de cadastro e listagem.
- **Banco de Dados:** instância **RDS MySQL** para persistência dos dados.
- **Hospedagem:** aplicação executada em uma **instância EC2** na AWS.

---

## 🏗️ Arquitetura

<img width="481" height="279" alt="Captura de tela 2025-11-04 232910" src="https://github.com/user-attachments/assets/3681e797-c6a1-42dd-835f-63f430ea1b01" />

---

## ⚙️ Tecnologias Utilizadas

- **Frontend:** HTML5, CSS3, JavaScript  
- **Backend:** Python 3 + Flask + MySQL Connector  
- **Banco de Dados:** Amazon RDS MySQL  
- **Hospedagem:** Amazon EC2  
- **Gerenciamento de Dependências:** pip  
- **Ferramentas de apoio:** Git, GitHub, VS Code  

---

## 💻 Backend (app.py)

```python
import json
import mysql.connector
from flask import Flask, request, jsonify
from flask_cors import CORS

app = Flask(__name__)
CORS(app)

db_config = {
    'host': 'rm-python-elainematos.cwx0oaakifpd.us-east-1.rds.amazonaws.com',
    'user': 'admin',
    'password': 'FiapAluno2023!',
    'database': 'db_alunos',
    'port': 3306
}

def insert_aluno(aluno):
    conn = mysql.connector.connect(**db_config)
    cursor = conn.cursor()
    query = """
        INSERT INTO tb_alunos
        (aluno_nome, aluno_email, aluno_telefone, aluno_endereco, aluno_curso, aluno_turma, aluno_ano)
        VALUES (%s, %s, %s, %s, %s, %s, %s)
    """
    values = (aluno['nome'], aluno['email'], aluno['telefone'], aluno['endereco'], aluno['curso'], aluno['turma'], aluno['ano'])
    cursor.execute(query, values)
    conn.commit()
    cursor.close()
    conn.close()

def fetch_alunos():
    conn = mysql.connector.connect(**db_config)
    cursor = conn.cursor(dictionary=True)
    cursor.execute("SELECT * FROM tb_alunos")
    result = cursor.fetchall()
    cursor.close()
    conn.close()
    return result

@app.route('/alunos', methods=['GET'])
def get_alunos():
    alunos = fetch_alunos()
    return jsonify(alunos)

@app.route('/cadastro', methods=['POST'])
def cadastro():
    aluno = request.get_json()
    if not aluno:
        return jsonify({'erro': 'Nenhum dado foi enviado ou JSON inválido'}), 400
    insert_aluno(aluno)
    return jsonify({'mensagem': 'Dados do aluno recebidos com sucesso!'})

@app.route('/')
def index():
    return 'Bem-vindo ao cadastro de alunos!'

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=5000, debug=True)

```
## 🌐 Frontend (index.html)

```
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cadastro de Alunos</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Cadastro de Alunos</h1>
        <div id="mensagem"></div>
        <form id="cadastro-aluno" method="post" action="http://3.235.106.254:5000/cadastro">
            <div class="form-group"><label>Nome:</label><input type="text" name="nome" required></div>
            <div class="form-group"><label>Email:</label><input type="email" name="email" required></div>
            <div class="form-group"><label>Telefone:</label><input type="tel" name="telefone" required></div>
            <div class="form-group"><label>Endereço:</label><input type="text" name="endereco" required></div>
            <div class="form-group"><label>Curso:</label><input type="text" name="curso" required></div>
            <div class="form-group"><label>Turma:</label><input type="text" name="turma" required></div>
            <div class="form-group"><label>Ano:</label><input type="number" name="ano" required></div>
            <button type="submit" class="submit-btn">Cadastrar</button>
        </form>
    </div>
    <script src="script.js"></script>
</body>
</html>

```
## ⚡ Script (script.js)

```
document.getElementById('cadastro-aluno').addEventListener('submit', async function(event) {
    event.preventDefault();

    const formData = {
        nome: this.nome.value,
        email: this.email.value,
        telefone: this.telefone.value,
        endereco: this.endereco.value,
        curso: this.curso.value,
        turma: this.turma.value,
        ano: this.ano.value
    };

    const response = await fetch('http://3.235.106.254:5000/cadastro', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
    });

    const result = await response.json();
    document.getElementById('mensagem').innerText = result.mensagem;
});
```
## 🧩 Estrutura do Projeto
```
aws-backend-front/
│
├── backend/
│   └── app.py
│
├── frontend/
│   ├── index.htmlS
│   ├── styles.css
│   └── script.js
│
└── README.md
```

## 👩‍🏫 Créditos

Este projeto foi realizado durante o curso de DevOps - Santander & Alura, sob orientação da professora Olivia Braga, como parte prática do laboratório AWS Backend-Front.

## 🌍 Autor

Elaine Matos
💼 LinkedIn: linkedin.com/in/elainematos

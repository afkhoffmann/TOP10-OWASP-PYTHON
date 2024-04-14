# <img src="../../imagens/TOP_10_Icons_Final_Injection.png" width="50px" style="vertical-align: middle;"> A03-Injection
Falhas de Injeção ocorrem quando dados não confiáveis são enviados a um interpretador como parte de um comando ou consulta, levando à execução não intencional. Seja injeção SQL, NoSQL ou de comando do sistema operacional, não validar e higienizar a entrada do usuário pode deixar as aplicações vulneráveis. Isso pode levar a vazamentos de dados, acesso não autorizado ou comprometimento completo do sistema. Implementar validação de entrada, usar consultas parametrizadas e aproveitar ferramentas de teste automatizado pode ajudar a identificar e prevenir vulnerabilidades de injeção.

# Exemplo Comprometido
Neste exemplo, o código é vulnerável a ataques de SQL Injection.

```python
from flask import Flask, request, jsonify
import sqlite3

app = Flask(__name__)

@app.route('/accountInfo', methods=['GET'])
def account_info():
    cust_id = request.args.get('id')
    
    # Construção da consulta SQL diretamente com dados não verificados
    query = "SELECT * FROM accounts WHERE custID='" + cust_id + "'"
    
    conn = sqlite3.connect('database.db')
    cursor = conn.cursor()
    
    # Execução da consulta SQL
    cursor.execute(query)
    
    # Recuperação dos resultados
    accounts = cursor.fetchall()
    
    conn.close()
    
    return jsonify(accounts)

if __name__ == '__main__':
    app.run(debug=True)
```

# Correção
Neste exemplo, o código foi corrigido utilizando consultas parametrizadas para evitar SQL Injection.

```python
from flask import Flask, request, jsonify
import sqlite3

app = Flask(__name__)

@app.route('/accountInfo', methods=['GET'])
def account_info():
    cust_id = request.args.get('id')
    
    # Utilização de consulta parametrizada para prevenir SQL Injection
    query = "SELECT * FROM accounts WHERE custID=?"
    
    conn = sqlite3.connect('database.db')
    cursor = conn.cursor()
    
    # Execução da consulta SQL parametrizada
    cursor.execute(query, (cust_id,))
    
    # Recuperação dos resultados
    accounts = cursor.fetchall()
    
    conn.close()
    
    return jsonify(accounts)

if __name__ == '__main__':
    app.run(debug=True)
```
No exemplo comprometido, o endpoint /accountInfo constrói uma consulta SQL diretamente com os dados fornecidos pelo usuário (cust_id), tornando-o vulnerável a ataques de SQL Injection.

Na correção, utilizamos consultas parametrizadas para prevenir SQL Injection. Isso é feito substituindo os valores dos parâmetros na consulta SQL por marcadores de posição (?) e fornecendo os valores separadamente. Isso impede que os valores fornecidos pelos usuários sejam interpretados como parte da consulta SQL, evitando assim a execução de comandos SQL maliciosos.

Esta correção garante que os dados fornecidos pelos usuários sejam tratados como dados e não como comandos SQL, protegendo a aplicação contra SQL Injection.

[← Voltar](../../README.md)
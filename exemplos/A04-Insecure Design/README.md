# <img src="../../imagens/TOP_10_Icons_Final_Insecure_Design.png" width="50px" style="vertical-align: middle;"> A04-Insecure Design
Design Inseguro representa falhas decorrentes de controles de segurança inadequados nas fases iniciais de arquitetura e design de uma aplicação. Esses não são meros problemas de implementação, mas falhas de design fundamentais que não podem ser corrigidas apenas com patches. Falta de modelagem de ameaças, padrões de design inseguros e falha em abordar perfis de risco empresarial podem resultar em aplicações inerentemente vulneráveis. Um design seguro é fundamental; sem ele, até as melhores implementações não podem fornecer proteção adequada.

# Exemplo Comprometido
Neste exemplo, o código possui um fluxo de recuperação de credenciais inseguro que utiliza perguntas e respostas para validação.

```python
from flask import Flask, request, jsonify
import sqlite3

app = Flask(__name__)

@app.route('/recoverCredentials', methods=['POST'])
def recover_credentials():
    username = request.json.get('username')
    security_question = request.json.get('security_question')
    security_answer = request.json.get('security_answer')
    
    # Verificação de segurança utilizando perguntas e respostas
    if security_question == "Qual é o nome do seu animal de estimação?" and security_answer == "Rex":
        conn = sqlite3.connect('database.db')
        cursor = conn.cursor()
        
        # Consulta SQL para recuperar a senha do usuário
        cursor.execute(f"SELECT password FROM users WHERE username='{username}'")
        
        # Recuperação da senha do usuário
        user = cursor.fetchone()
        
        conn.close()
        
        if user:
            return jsonify({"password": user[0]}), 200
        else:
            return jsonify({"error": "Usuário não encontrado"}), 404
    else:
        return jsonify({"error": "Resposta de segurança incorreta"}), 400

if __name__ == '__main__':
    app.run(debug=True)
```

# Correção
Neste exemplo, o código foi corrigido utilizando um fluxo de recuperação de credenciais mais seguro sem perguntas e respostas.

```python
from flask import Flask, request, jsonify
import sqlite3

app = Flask(__name__)

@app.route('/recoverCredentials', methods=['POST'])
def recover_credentials():
    username = request.json.get('username')
    
    # Consulta SQL segura para recuperar a senha do usuário
    conn = sqlite3.connect('database.db')
    cursor = conn.cursor()
    
    cursor.execute("SELECT password FROM users WHERE username=?", (username,))
    
    # Recuperação da senha do usuário
    user = cursor.fetchone()
    
    conn.close()
    
    if user:
        return jsonify({"password": user[0]}), 200
    else:
        return jsonify({"error": "Usuário não encontrado"}), 404

if __name__ == '__main__':
    app.run(debug=True)
```
No exemplo comprometido, o endpoint /recoverCredentials utiliza um fluxo de recuperação de credenciais inseguro que utiliza perguntas e respostas para validação. Este método é considerado inseguro porque as perguntas e respostas podem ser facilmente adivinhadas ou obtidas por meio de engenharia social, tornando o sistema vulnerável a ataques.

Na correção, removemos a validação por perguntas e respostas e substituímos por uma consulta SQL parametrizada para recuperar a senha do usuário com base no nome de usuário fornecido. Isso elimina a necessidade de perguntas e respostas de segurança, tornando o sistema mais seguro contra ataques de recuperação de credenciais.

Esta correção demonstra a importância de considerar o design seguro desde o início do desenvolvimento de um sistema, evitando vulnerabilidades decorrentes de decisões de design inseguras.

[← Voltar](../../README.md)
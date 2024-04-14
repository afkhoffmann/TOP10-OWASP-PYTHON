# <img src="../../imagens/TOP_10_Icons_Final_Identification_and_Authentication_Failures.png" width="50px" style="vertical-align: middle;"> A07-Identification and Authentication Failures
Falhas de Identificação e Autenticação giram em torno de fraquezas na confirmação de identidades de usuário e gerenciamento de sessões de forma segura. De senhas fracas a autenticação multifator inadequada e gerenciamento de sessão, essas vulnerabilidades podem permitir que atacantes obtenham acesso não autorizado. Implementar políticas de senha fortes, autenticação multifator robusta e controles de gerenciamento de sessão sólidos podem ajudar a prevenir acesso não autorizado e proteger contas de usuário.

# Exemplo Comprometido
Neste exemplo, a aplicação Flask permite acesso a uma página protegida sem autenticação adequada.

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/admin', methods=['GET'])
def admin_page():
    token = request.args.get('token')
    
    if token == '123456':
        return jsonify({"message": "Welcome to admin page"}), 200
    else:
        return jsonify({"message": "Unauthorized"}), 401

if __name__ == '__main__':
    app.run(debug=True)
```


# Correção
Neste exemplo, a aplicação Flask implementa autenticação e validação de token adequadas.

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

def authenticate(token):
    valid_tokens = ['secure_token_1', 'secure_token_2']
    
    if token in valid_tokens:
        return True
    else:
        return False

@app.route('/admin', methods=['GET'])
def admin_page():
    token = request.args.get('token')
    
    if authenticate(token):
        return jsonify({"message": "Welcome to admin page"}), 200
    else:
        return jsonify({"message": "Unauthorized"}), 401

if __name__ == '__main__':
    app.run(debug=True)
```
No exemplo comprometido, a aplicação Flask permite acesso à página de administração com base em um token simples fornecido via parâmetro de consulta. Isso é uma falha de autenticação, pois não verifica adequadamente a identidade do usuário.

Na correção, implementamos uma função authenticate para validar o token fornecido. Se o token estiver na lista de tokens válidos, a função retorna True, caso contrário, retorna False. A função admin_page agora verifica o token usando a função authenticate antes de conceder acesso à página de administração.

Esta correção ajuda a mitigar as vulnerabilidades associadas às falhas de identificação e autenticação, garantindo que apenas usuários autenticados e autorizados possam acessar recursos protegidos da aplicação Flask.

[← Voltar](../../README.md)
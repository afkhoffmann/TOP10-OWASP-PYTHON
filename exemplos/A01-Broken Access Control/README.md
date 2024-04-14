# <img src="../../imagens/TOP_10_Icons_Final_Broken_Access_Control.png" width="50px" style="vertical-align: middle;"> A01-Broken Access Control 
Controle de Acesso Quebrado trata da correta implementação de permissões de usuário. Quando isso falha, usuários podem exceder seus privilégios, acessando dados ou funcionalidades não autorizadas. Isso pode acontecer devido a lógicas de controle de acesso defeituosas, contornando verificações ou referências diretas a objetos inseguras. Atacantes podem explorar essas falhas para visualizar contas de outros, elevar privilégios ou acessar APIs sem controles adequados. Implementar um mecanismo robusto de controle de acesso, aderir ao princípio do mínimo privilégio e validar permissões de usuário em cada etapa são passos cruciais para mitigar esse risco.

# Exemplo Comprometido
Neste exemplo, não há verificação adequada de acesso, permitindo acesso não autorizado.

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

# Dados fictícios de contas
accounts = {
    'user1': {
        'balance': 1000,
        'account_number': '123456789'
    },
    'user2': {
        'balance': 500,
        'account_number': '987654321'
    }
}

@app.route('/accountInfo', methods=['GET'])
def account_info():
    acct = request.args.get('acct')
    return jsonify(accounts.get(acct, 'Conta não encontrada'))

if __name__ == '__main__':
    app.run(debug=True)
```

# Correção
Neste exemplo, a verificação adequada de acesso é implementada para evitar acesso não autorizado.

```python
from flask import Flask, request, jsonify, abort

app = Flask(__name__)

# Dados fictícios de contas
accounts = {
    'user1': {
        'balance': 1000,
        'account_number': '123456789'
    },
    'user2': {
        'balance': 500,
        'account_number': '987654321'
    }
}

@app.route('/accountInfo', methods=['GET'])
def account_info():
    acct = request.args.get('acct')
    
    # Verificação de acesso
    if 'user' not in request.headers or request.headers['user'] != acct:
        abort(403, description="Acesso negado")
    
    return jsonify(accounts.get(acct, 'Conta não encontrada'))

if __name__ == '__main__':
    app.run(debug=True)
```
No exemplo comprometido, o endpoint /accountInfo permite que qualquer usuário acesse informações de conta fornecendo um parâmetro acct. Isso é um exemplo clássico de violação de controle de acesso, onde não há verificação adequada para garantir que o usuário está autorizado a acessar essa informação.

Na correção, adicionamos uma verificação de acesso que compara o header 'user' na solicitação com o parâmetro acct. Se eles não corresponderem, uma resposta HTTP 403 (Acesso Negado) é retornada, impedindo o acesso não autorizado às informações da conta.

[← Voltar](../../README.md)
# <img src="../../imagens/TOP_10_Icons_Final_Crypto_Failures.png" width="50px" style="vertical-align: middle;"> A02-Cryptographic Failures
Falhas Criptográficas focam na proteção de dados sensíveis. Essas vulnerabilidades surgem quando os dados não são adequadamente protegidos durante a transmissão ou armazenamento. Usar algoritmos criptográficos fracos ou obsoletos, gerenciamento inadequado de chaves ou não impor criptografia onde necessário pode levar a violações de dados. As organizações precisam identificar quais dados precisam de proteção, garantir o uso de algoritmos criptográficos robustos, gerenciar chaves de forma segura e validar certificados de servidor para evitar acesso e exposição não autorizados. 

# Exemplo Comprometido
Neste exemplo, as informações sensíveis são armazenadas e transmitidas de forma insegura.

```python
from flask import Flask, request, jsonify
import hashlib

app = Flask(__name__)

# Dados fictícios de contas
accounts = {
    'user1': {
        'balance': 1000,
        'credit_card_number': '1234567890123456'
    }
}

@app.route('/accountInfo', methods=['GET'])
def account_info():
    user = request.args.get('user')
    
    if user in accounts:
        # Armazenamento da informação do cartão de crédito de forma insegura
        credit_card_number = accounts[user]['credit_card_number']
        
        # Envio da informação do cartão de crédito sem criptografia
        return jsonify({'credit_card_number': credit_card_number})
    else:
        return jsonify({'message': 'Usuário não encontrado'}), 404

if __name__ == '__main__':
    app.run(debug=True)
```

# Correção
Neste exemplo, as informações sensíveis são armazenadas e transmitidas de forma segura.

```python
from flask import Flask, request, jsonify
import hashlib
from cryptography.fernet import Fernet

app = Flask(__name__)

# Chave de criptografia (Deve ser armazenada de forma segura)
key = Fernet.generate_key()
cipher = Fernet(key)

# Dados fictícios de contas
accounts = {
    'user1': {
        'balance': 1000,
        'credit_card_number': '1234567890123456'
    }
}

@app.route('/accountInfo', methods=['GET'])
def account_info():
    user = request.args.get('user')
    
    if user in accounts:
        # Armazenamento da informação do cartão de crédito de forma segura
        credit_card_number = accounts[user]['credit_card_number'].encode()
        encrypted_credit_card = cipher.encrypt(credit_card_number)
        
        # Envio da informação do cartão de crédito criptografada
        return jsonify({'encrypted_credit_card': encrypted_credit_card.decode()})
    else:
        return jsonify({'message': 'Usuário não encontrado'}), 404

if __name__ == '__main__':
    app.run(debug=True)
```
No exemplo comprometido, o endpoint /accountInfo retorna o número do cartão de crédito de um usuário sem qualquer forma de criptografia, tornando os dados sensíveis vulneráveis a ataques.

Na correção, utilizamos a biblioteca cryptography para criptografar o número do cartão de crédito antes de armazená-lo ou transmiti-lo. A chave de criptografia é gerada pelo Fernet e deve ser armazenada de forma segura para garantir a segurança dos dados.

Esta correção garante que as informações sensíveis, como números de cartão de crédito, sejam armazenadas e transmitidas de forma segura, protegendo os dados contra exposição indevida.

[← Voltar](../../README.md)
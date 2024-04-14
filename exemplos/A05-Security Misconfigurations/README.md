# <img src="../../imagens/TOP_10_Icons_Final_Security_Misconfiguration.png" width="50px" style="vertical-align: middle;"> A05-Security Misconfigurations
Configuração de Segurança Incorreta surge de configurações inadequadas em todo o stack da aplicação, incluindo servidores, frameworks e bancos de dados. Configurações padrão, recursos desnecessários e software desatualizado podem expor as aplicações a várias ameaças. Garantir configurações padrão seguras, atualizações regulares e seguir o princípio do mínimo privilégio pode reduzir significativamente a superfície de ataque e proteger contra possíveis violações.

# Exemplo Comprometido
Neste exemplo, o servidor Flask está usando as configurações padrão, sem hardening adequado e envia mensagens de erro detalhadas para os usuários.

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/getSecretData', methods=['GET'])
def get_secret_data():
    secret_data = "super_secret_data"
    
    # Simulação de erro para retornar uma mensagem de erro detalhada
    if request.headers.get('Authorization') != 'Bearer my_secret_token':
        raise Exception("Acesso negado: Token inválido!")
    
    return jsonify({"data": secret_data}), 200

if __name__ == '__main__':
    app.run(debug=True)
```

# Correção
Neste exemplo, o servidor Flask foi configurado com hardening adequado, desabilitação de mensagens de erro detalhadas e headers de segurança.

```python
from flask import Flask, request, jsonify
from flask_talisman import Talisman

app = Flask(__name__)
Talisman(app, content_security_policy=None, force_https=True, strict_transport_security=True)

@app.route('/getSecretData', methods=['GET'])
def get_secret_data():
    secret_data = "super_secret_data"
    
    # Verificação segura do token de autorização
    if request.headers.get('Authorization') != 'Bearer my_secret_token':
        return jsonify({"error": "Acesso negado"}), 403
    
    return jsonify({"data": secret_data}), 200

if __name__ == '__main__':
    app.run(debug=True)
```
No exemplo comprometido, o servidor Flask está usando as configurações padrão, sem hardening adequado e envia mensagens de erro detalhadas para os usuários. Isso pode expor informações sensíveis e vulnerabilidades do sistema aos atacantes.

Na correção, aplicamos as seguintes medidas de segurança:

- Utilização da extensão flask_talisman para adicionar políticas de segurança HTTP ao aplicativo, incluindo force_https para forçar o uso de HTTPS e strict_transport_security para habilitar a Política de Segurança de Transporte Estrito (HSTS).
- Desabilitação de mensagens de erro detalhadas para evitar a exposição de informações sensíveis.
- Verificação segura do token de autorização para acessar os dados secretos, retornando um código de status HTTP 403 (Acesso Negado) em caso de token inválido.

Essas correções ajudam a mitigar as vulnerabilidades de misconfiguração de segurança no servidor Flask, protegendo contra possíveis ataques que exploram configurações inseguras.

[← Voltar](../../README.md)
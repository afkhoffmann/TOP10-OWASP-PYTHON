# <img src="../../imagens/TOP_10_Icons_Final_SSRF.png" width="50px" style="vertical-align: middle;"> A10-Server-Side Request Forgery
Vulnerabilidades SSRF ocorrem quando uma aplicação busca um recurso remoto sem validar corretamente a URL fornecida pelo usuário. Isso pode permitir que atacantes iniciem solicitações a sistemas internos, contornando firewalls e listas de controle de acesso à rede. Com a crescente complexidade das arquiteturas modernas e o uso de serviços em nuvem, a gravidade e a incidência de vulnerabilidades SSRF estão aumentando. Validar e sanitizar corretamente as URLs fornecidas pelo usuário, impor regras rigorosas de firewall e segmentar o acesso à rede podem ajudar a mitigar esse risco.

# Exemplo Comprometido
Neste exemplo, a aplicação Flask faz uma requisição para uma URL fornecida pelo usuário sem validação adequada.

```python
from flask import Flask, request
import requests

app = Flask(__name__)

@app.route('/')
def index():
    url = request.args.get('url')
    
    # Faz a requisição para a URL fornecida pelo usuário
    response = requests.get(url)
    
    return response.text

if __name__ == '__main__':
    app.run(debug=True)
```

# Correção
Neste exemplo, a aplicação Flask valida e sanitiza a URL fornecida pelo usuário antes de fazer a requisição.

```python
from flask import Flask, request
import requests
from urllib.parse import urlparse

app = Flask(__name__)

@app.route('/')
def index():
    url = request.args.get('url')
    
    # Valida a URL fornecida pelo usuário
    parsed_url = urlparse(url)
    
    if parsed_url.scheme not in ['http', 'https']:
        return 'URL inválida.', 400
    
    # Faz a requisição para a URL fornecida pelo usuário
    response = requests.get(url)
    
    return response.text

if __name__ == '__main__':
    app.run(debug=True)
```
No exemplo comprometido, a aplicação Flask faz uma requisição para uma URL fornecida pelo usuário sem realizar validação adequada. Isso permite que um atacante force a aplicação a enviar uma requisição para um destino inesperado, mesmo quando protegida por firewall, VPN ou outra forma de lista de controle de acesso à rede (ACL).

Na correção, utilizamos a função urlparse para validar a URL fornecida pelo usuário. Verificamos se o esquema da URL é http ou https. Caso contrário, retornamos um erro informando que a URL é inválida.

Esta correção ajuda a mitigar as vulnerabilidades associadas ao SSRF, garantindo que a aplicação Flask valide e sanitize corretamente as URLs fornecidas pelos usuários antes de fazer qualquer requisição.

[← Voltar](../../README.md)
# <img src="../../imagens/TOP_10_Icons_Final_Security_Logging_and_Monitoring_Failures.png" width="50px" style="vertical-align: middle;"> A09-Security Logging and Monitoring Failures
Logging e monitoramento eficazes são cruciais para detectar, escalar e responder a violações de segurança. Logging insuficiente, mensagens de log pouco claras e monitoramento inadequado podem atrasar a detecção e resposta a incidentes, dando aos atacantes mais tempo para operar sem serem detectados. Implementar logging abrangente, configurar mecanismos de alerta e revisar regularmente os logs podem ajudar as organizações a detectar e responder a incidentes de segurança prontamente.

# Exemplo Comprometido
Neste exemplo, a aplicação Flask não registra eventos importantes em seus logs.

```python
from flask import Flask, request

app = Flask(__name__)

@app.route('/')
def index():
    username = request.args.get('username')
    password = request.args.get('password')
    
    # Realiza a autenticação
    if username == 'admin' and password == 'admin123':
        return 'Login bem-sucedido!'
    else:
        return 'Credenciais inválidas.'

if __name__ == '__main__':
    app.run(debug=True)
```

# Correção
Neste exemplo, a aplicação Flask registra eventos importantes em seus logs.

```python
from flask import Flask, request
import logging

app = Flask(__name__)

# Configuração do logging
logging.basicConfig(filename='app.log', level=logging.INFO,
                    format='%(asctime)s - %(levelname)s - %(message)s')

@app.route('/')
def index():
    username = request.args.get('username')
    password = request.args.get('password')
    
    # Realiza a autenticação
    if username == 'admin' and password == 'admin123':
        logging.info(f"Login bem-sucedido para o usuário: {username}")
        return 'Login bem-sucedido!'
    else:
        logging.warning(f"Tentativa de login falhou para o usuário: {username}")
        return 'Credenciais inválidas.'

if __name__ == '__main__':
    app.run(debug=True)
```
No exemplo comprometido, a aplicação Flask não registra eventos importantes, como tentativas de login bem-sucedidas ou falhas, em seus logs. Isso pode tornar difícil detectar e responder a ataques em tempo real.

Na correção, utilizamos o módulo logging do Python para configurar o registro de eventos importantes em um arquivo de log (app.log). Adicionamos mensagens de log para registrar tentativas de login bem-sucedidas e falhas.

Esta correção ajuda a mitigar as vulnerabilidades associadas às falhas de logging e monitoring, garantindo que a aplicação Flask registre eventos importantes que podem ser úteis para detecção, escalonamento e resposta a ataques em tempo real.

[← Voltar](../../README.md)
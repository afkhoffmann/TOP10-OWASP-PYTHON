# <img src="../../imagens/TOP_10_Icons_Final_Software_and_Data_Integrity_Failures.png" width="50px" style="vertical-align: middle;"> A08-Software and Data Integrity Failures
Falhas de Integridade de Software e Dados estão relacionadas a vulnerabilidades decorrentes de código e infraestrutura comprometidos que não protegem contra violações de integridade. Pipelines CI/CD inseguras, dependência de fontes não confiáveis e verificação insuficiente de integridade podem levar à introdução de código malicioso ou acesso não autorizado. Garantir práticas de desenvolvimento seguro, verificar a integridade de todo o código e componentes e proteger os pipelines CI/CD são fundamentais para manter a confiabilidade do software e dos dados.

# Exemplo Comprometido
Neste exemplo, a aplicação Flask utiliza uma biblioteca de terceiros sem verificar sua integridade.

```python
from flask import Flask, jsonify
import untrusted_library

app = Flask(__name__)

@app.route('/')
def hello():
    result = untrusted_library.unsafe_function()
    return jsonify({"message": result})

if __name__ == '__main__':
    app.run(debug=True)
```

# Correção
Neste exemplo, a aplicação Flask utiliza uma biblioteca de terceiros com verificação de sua integridade.

```python
from flask import Flask, jsonify
import hashlib
import untrusted_library

app = Flask(__name__)

def verify_integrity():
    # Hash da biblioteca de terceiros verificada
    expected_hash = "hash_da_biblioteca_verificada"
    
    # Calcula o hash da biblioteca de terceiros atual
    current_hash = hashlib.sha256(open("untrusted_library.py", "rb").read()).hexdigest()
    
    # Compara os hashes
    if current_hash == expected_hash:
        return True
    else:
        return False

@app.route('/')
def hello():
    if verify_integrity():
        result = untrusted_library.safe_function()
        return jsonify({"message": result})
    else:
        return jsonify({"message": "Integrity check failed"}), 500

if __name__ == '__main__':
    app.run(debug=True)
```

No exemplo comprometido, a aplicação Flask importa e utiliza uma biblioteca de terceiros (untrusted_library) sem verificar sua integridade. Isso pode resultar em uma potencial vulnerabilidade de integridade de software, onde um atacante poderia fornecer uma versão maliciosa dessa biblioteca.

Na correção, adicionamos uma função verify_integrity que calcula o hash da biblioteca de terceiros atual e compara com um hash esperado. Se os hashes forem diferentes, a função retorna False, indicando uma falha na verificação de integridade. A função hello agora verifica a integridade da biblioteca usando verify_integrity antes de chamar qualquer função da biblioteca.

Esta correção ajuda a mitigar as vulnerabilidades associadas às falhas de integridade de software e dados, garantindo que apenas bibliotecas e componentes de software verificados e confiáveis sejam utilizados pela aplicação Flask.

[← Voltar](../../README.md)
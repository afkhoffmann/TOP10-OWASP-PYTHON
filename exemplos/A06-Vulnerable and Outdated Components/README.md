# <img src="../../imagens/TOP_10_Icons_Final_Vulnerable_Outdated_Components.png" width="50px" style="vertical-align: middle;"> A06-Vulnerable and Outdated Components
Este risco envolve o uso de componentes, bibliotecas ou frameworks desatualizados ou conhecidos por terem vulnerabilidades. Sem um inventário adequado e varreduras regulares de vulnerabilidades, as organizações podem introduzir inadvertidamente fraquezas em suas aplicações. Manter um inventário atualizado de todos os componentes, realizar varreduras regulares de vulnerabilidades e aplicar patches ou atualizações prontamente são essenciais para mitigar esse risco.

# Exemplo Comprometido
Neste exemplo, a aplicação Flask utiliza uma versão desatualizada de uma biblioteca, que possui uma vulnerabilidade conhecida.

```python
from flask import Flask, jsonify
from outdated_library import vulnerable_function

app = Flask(__name__)

@app.route('/getVulnerableData', methods=['GET'])
def get_vulnerable_data():
    data = vulnerable_function()
    
    return jsonify({"data": data}), 200

if __name__ == '__main__':
    app.run(debug=True)
```


# Correção
Neste exemplo, a aplicação Flask utiliza uma biblioteca atualizada e segura.

```python
from flask import Flask, jsonify
import secure_library

app = Flask(__name__)

@app.route('/getSecureData', methods=['GET'])
def get_secure_data():
    data = secure_library.secure_function()
    
    return jsonify({"data": data}), 200

if __name__ == '__main__':
    app.run(debug=True)
```
No exemplo comprometido, a aplicação Flask utiliza uma versão desatualizada de uma biblioteca (outdated_library) que possui uma vulnerabilidade conhecida. Isso pode expor a aplicação a riscos de segurança, como ataques de execução de código remoto.

Na correção, substituímos a biblioteca desatualizada por uma versão atualizada e segura (secure_library). É crucial manter todas as bibliotecas e componentes da aplicação atualizados para evitar vulnerabilidades conhecidas e exploradas por atacantes.

Esta correção ajuda a mitigar as vulnerabilidades associadas ao uso de componentes desatualizados e vulneráveis, mantendo a aplicação Flask segura contra possíveis ataques.

[← Voltar](../../README.md)
# Laboratório Anchieta - laboratório didático de cybersegurança

Projeto intencionalmente inseguro para uso exclusivamente educacional, em ambiente isolado.


## Como executar
```bash
python -m venv .venv
# Linux/macOS
source .venv/bin/activate
# Windows
# .venv\Scripts\activate

pip install -r requirements.txt
python app.py
```

Acesse:
- http://127.0.0.1:5000


### Objetivo didático
Essa vulnerabilidade simula um erro comum de operação: deixar um dump de banco ou backup acessível no servidor web.
Assim, os estudantes podem identificar:
- exposição indevida de dados internos
- riscos de enumeração e coleta de informações
- impacto de backups esquecidos em diretórios públicos


### Observação
Esta versão não contém XSS explorável. Ela foi preparada para ensinar reconhecimento do risco, impacto e mitigação segura, sem disponibilizar uma falha ativa no sistema.

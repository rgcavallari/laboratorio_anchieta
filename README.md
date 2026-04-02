# Portal Aurora - laboratório didático de cybersegurança

Projeto intencionalmente inseguro para uso exclusivamente educacional, em ambiente isolado.

## O que foi incluído
- Banner de consentimento de cookies
- Registro didático de IP, rota, método e User-Agent em SQLite
- Painel administrativo com visualização dos últimos acessos
- Backend em Flask
- Interface estilizada
- Superfícies expostas para enumeração e análise
- Arquivo oculto com a prova dentro de `/backup/`

## Fragilidades intencionais do laboratório
- Diretório de backup acessível publicamente
- `robots.txt` revelando caminhos sensíveis
- Credenciais fracas e previsíveis
- Reflected XSS na busca (`/search?q=...`)
- IDOR simples em `/profile?id=` e `/report/<id>`
- Página de status com excesso de informações

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

## Sugestão de uso em aula
Deixe a aplicação rodando em uma VM ou máquina local da rede do laboratório.
Depois, peça aos estudantes que façam:
- reconhecimento do alvo
- enumeração do serviço web
- identificação das fragilidades
- localização do arquivo com a prova

## Importante
Use apenas em laboratório, rede isolada ou máquina virtual.
Não publique este projeto em ambiente público.


## Observação sobre cookies e IP
Para saber o IP de quem acessa o site, o cookie não é obrigatório: o IP é visível no backend pela requisição HTTP.
Neste projeto, o cookie foi adicionado para registrar a escolha de consentimento do visitante, enquanto os logs são salvos no banco `access_logs.db`.

## Como funciona
- O backend registra IP, rota, método, data/hora e User-Agent
- O cookie `aurora_cookie_consent` guarda se o visitante aceitou ou recusou
- O painel `/access-log` mostra os últimos 100 acessos para o administrador


## Novas funcionalidades administrativas
- Cadastro de novos usuários pelo painel `/admin/users`
- Redefinição de senha de usuários existentes
- Exclusão de contas (com bloqueio para não excluir o usuário logado)
- Persistência simples em `users.json`

> Observação: as senhas continuam em texto puro de propósito, para manter o laboratório didático e gerar discussões sobre más práticas de segurança.


## Nova vulnerabilidade adicionada
Foi incluído um banco de dados SQLite com informações totalmente fictícias do ambiente interno.

### Arquivos e rotas
- Banco principal: `corp_data.db`
- Cópia exposta propositalmente: `static/backup/corp_data_backup.db`
- Página de consulta: `/records`
- Página de chamados: `/tickets`

### Objetivo didático
Essa vulnerabilidade simula um erro comum de operação: deixar um dump de banco ou backup acessível no servidor web.
Assim, os estudantes podem identificar:
- exposição indevida de dados internos
- riscos de enumeração e coleta de informações
- impacto de backups esquecidos em diretórios públicos


## Vulnerabilidades indiretas adicionadas

- Arquivo de configuração exposto (`/static/config/dev-config.json`)
- Logs acessíveis (`/static/logs/error.log`)
- Endpoint interno ativo (`/internal-api`)
- Comentários no HTML com pistas
- Notas internas acessíveis (`/static/internal/notes.txt`)

Esses elementos simulam falhas reais sem permitir exploração destrutiva.


## Trilha didática sobre XSS
Foi adicionada uma trilha segura de análise:
- `/xss-lab`: recebe um payload e o exibe com escape seguro
- `/code-review/search`: mostra um trecho didático de revisão de código
- `/security-notes`: reúne notas do time de AppSec
- `PROVA_ENADE_XSS.md`: prova estilo ENADE baseada no tema

### Observação
Esta versão não contém XSS explorável. Ela foi preparada para ensinar reconhecimento do risco, impacto e mitigação segura, sem disponibilizar uma falha ativa no sistema.

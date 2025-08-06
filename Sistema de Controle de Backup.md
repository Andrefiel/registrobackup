# Sistema de Controle de Backup

Este é um sistema completo para controle e registro de backups, desenvolvido para auxiliar na organização e auditoria de processos de backup. Ele oferece funcionalidades de registro detalhado, gestão de usuários, geração de relatórios e um sistema inovador de assinatura digital.

## Funcionalidades Principais

-   **Registro de Backups**: Interface intuitiva para registrar informações essenciais de cada backup, incluindo data, hora de início e fim, e responsáveis.
-   **Gestão de Usuários**: Funcionalidades para o administrador gerenciar usuários, incluindo cadastro e alteração de senhas, com controle de acesso baseado em perfis.
-   **Assinatura Digital**: Opção de coletar assinaturas dos responsáveis diretamente na tela (via touch ou mouse) ou através de upload de arquivos de imagem.
-   **Geração de Relatórios Mensais**: Relatórios detalhados em PDF com estatísticas e tempo gasto por backup, facilitando a análise e auditoria.
-   **Upload de Evidências**: Campo para anexar arquivos de evidência relacionados ao backup.
-   **Campo "Criado em"**: Registro automático da data de criação do backup, não editável.
-   **Autenticação Segura**: Sistema de login para garantir o acesso apenas a usuários autorizados.
-   **Conteinerização com Docker**: Aplicação totalmente conteinerizada para fácil implantação e escalabilidade.

## Tecnologias Utilizadas

-   **Backend**: Python 3.x com Flask
-   **Banco de Dados**: SQLite (para simplicidade, pode ser configurado para PostgreSQL/MySQL em produção)
-   **Frontend**: React.js com Vite
-   **Estilização**: Tailwind CSS e Shadcn/ui
-   **Assinatura Digital**: HTML Canvas
-   **Geração de PDF**: ReportLab
-   **Conteinerização**: Docker e Docker Compose

## Instalação e Uso (Docker)

Para instalar e rodar o sistema utilizando Docker e Docker Compose, siga os passos no arquivo `INSTALL.md`.

## Estrutura do Projeto

```
backup-control-system/
├── src/                  # Código fonte do backend Flask
│   ├── main.py           # Aplicação Flask principal
│   ├── models/           # Modelos de banco de dados (User, Backup)
│   ├── routes/           # Rotas da API (user, backup, reports, statistics)
│   └── static/           # Arquivos estáticos do frontend (build do React)
├── frontend/             # Código fonte do frontend React
│   ├── public/           # Arquivos públicos
│   ├── src/              # Componentes React, lógica, etc.
│   └── ...
├── uploads/              # Diretório para uploads de assinaturas e evidências
├── database/             # Diretório para o arquivo do banco de dados SQLite
├── logs/                 # Diretório para logs da aplicação
├── Dockerfile            # Definição da imagem Docker do backend
├── docker-compose.yml    # Orquestração dos serviços Docker
├── requirements.txt      # Dependências Python
├── .dockerignore         # Arquivos e diretórios a serem ignorados pelo Docker
├── README.md             # Este arquivo
└── INSTALL.md            # Guia de instalação detalhado
```

## Contribuição

Sinta-se à vontade para contribuir com melhorias, reportar bugs ou sugerir novas funcionalidades. Abra uma issue ou envie um pull request no repositório.

## Licença

Este projeto está licenciado sob a [Licença MIT](LICENSE).


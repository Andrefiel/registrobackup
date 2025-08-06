# Guia de Instalação - Sistema de Controle de Backup

Este guia fornece instruções detalhadas para instalar e configurar o Sistema de Controle de Backup usando Docker.

## Pré-requisitos

Antes de começar, certifique-se de ter os seguintes softwares instalados em seu sistema:

- **Docker**: Versão 20.10 ou superior
- **Docker Compose**: Versão 1.29 ou superior
- **Git**: Para clonar o repositório (opcional)

### Instalação do Docker (Ubuntu/Debian)

```bash
# Atualizar o sistema
sudo apt update

# Instalar dependências
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release

# Adicionar chave GPG oficial do Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Adicionar repositório do Docker
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Instalar Docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Adicionar usuário ao grupo docker
sudo usermod -aG docker $USER

# Reiniciar sessão ou executar
newgrp docker
```

## Instalação do Sistema

### Método 1: Usando arquivo compactado (Recomendado)

1. **Extrair o arquivo do sistema**:
   ```bash
   tar -xzf sistema-controle-backup.tar.gz
   cd backup-control-system
   ```

2. **Construir e iniciar os serviços**:
   ```bash
   docker-compose up -d --build
   ```

### Método 2: Clonando do repositório

1. **Clonar o repositório**:
   ```bash
   git clone <url-do-repositorio>
   cd backup-control-system
   ```

2. **Construir e iniciar os serviços**:
   ```bash
   docker-compose up -d --build
   ```

## Configuração

### Estrutura de Diretórios

O sistema criará automaticamente os seguintes diretórios para persistência de dados:

```
backup-control-system/
├── uploads/          # Assinaturas e evidências
├── database/         # Banco de dados SQLite
└── logs/            # Logs da aplicação
```

### Variáveis de Ambiente

As principais variáveis de ambiente estão configuradas no `docker-compose.yml`:

- `FLASK_ENV=production`: Ambiente de produção
- `SECRET_KEY`: Chave secreta para sessões (altere em produção)

### Portas

O sistema utiliza as seguintes portas:

- **5000**: Interface web principal
- **5432**: PostgreSQL (se configurado)

## Primeiro Acesso

1. **Aguardar inicialização**:
   ```bash
   # Verificar status dos containers
   docker-compose ps
   
   # Verificar logs se necessário
   docker-compose logs -f
   ```

2. **Acessar o sistema**:
   - Abra o navegador e acesse: `http://localhost:5000`
   - **Usuário padrão**: `admin`
   - **Senha padrão**: `123456`

3. **Primeira configuração**:
   - Acesse a aba "Usuários"
   - Altere a senha do administrador
   - Configure o logo da empresa (se necessário)
   - Crie usuários adicionais conforme necessário

## Funcionalidades Principais

### 1. Registro de Backups

- **Data do Backup**: Campo obrigatório para data
- **Hora de Início/Fim**: Campos para controle de tempo
- **Responsáveis**: Nomes dos responsáveis pelo backup e validação
- **Assinaturas**: Opção de upload ou assinatura digital
- **Evidências**: Upload opcional de arquivos comprobatórios

### 2. Assinatura Digital

O sistema oferece duas opções para coleta de assinaturas:

- **Upload de Arquivo**: Envio de imagem da assinatura
- **Assinatura Digital**: Desenho direto na tela usando canvas HTML5

Para usar a assinatura digital:
1. Selecione "Assinatura Digital" no dropdown
2. Clique em "Criar Assinatura Digital"
3. Desenhe a assinatura no canvas
4. Clique em "Salvar"

### 3. Gestão de Usuários

Funcionalidades disponíveis apenas para administradores:

- **Alterar Senha**: Modificar senha do usuário logado
- **Criar Usuários**: Cadastrar novos usuários no sistema
- **Listar Usuários**: Visualizar todos os usuários cadastrados
- **Deletar Usuários**: Remover usuários (exceto admin)

### 4. Relatórios

O sistema gera relatórios mensais em PDF contendo:

- **Estatísticas Gerais**: Total de backups, tempo médio
- **Backups por Responsável**: Distribuição por pessoa
- **Tempo Gasto**: Duração de cada backup
- **Gráficos**: Visualizações dos dados

## Comandos Úteis

### Gerenciamento do Sistema

```bash
# Iniciar o sistema
docker-compose up -d

# Parar o sistema
docker-compose down

# Reiniciar o sistema
docker-compose restart

# Ver logs em tempo real
docker-compose logs -f

# Atualizar o sistema
docker-compose down
docker-compose pull
docker-compose up -d --build

# Backup do banco de dados
docker-compose exec backup-control-system cp /app/database/backup_system.db /app/database/backup_$(date +%Y%m%d_%H%M%S).db
```

### Manutenção

```bash
# Limpar containers parados
docker container prune

# Limpar imagens não utilizadas
docker image prune

# Limpar volumes não utilizados
docker volume prune

# Verificar uso de espaço
docker system df
```

## Solução de Problemas

### Container não inicia

1. **Verificar logs**:
   ```bash
   docker-compose logs backup-control-system
   ```

2. **Verificar portas em uso**:
   ```bash
   sudo netstat -tulpn | grep :5000
   ```

3. **Reconstruir container**:
   ```bash
   docker-compose down
   docker-compose up -d --build --force-recreate
   ```

### Problemas de permissão

```bash
# Ajustar permissões dos diretórios
sudo chown -R $USER:$USER uploads/ database/ logs/
chmod -R 755 uploads/ database/ logs/
```

### Reset completo do sistema

```bash
# ATENÇÃO: Isso apagará todos os dados
docker-compose down -v
sudo rm -rf uploads/ database/ logs/
docker-compose up -d --build
```

## Segurança

### Recomendações para Produção

1. **Alterar credenciais padrão**:
   - Mude a senha do admin imediatamente
   - Altere a `SECRET_KEY` no docker-compose.yml

2. **Configurar HTTPS**:
   - Use um proxy reverso (nginx/traefik)
   - Configure certificados SSL

3. **Backup regular**:
   - Configure backup automático do diretório `database/`
   - Mantenha backups das assinaturas em `uploads/`

4. **Monitoramento**:
   - Configure logs centralizados
   - Monitore uso de recursos

## Suporte

Para suporte técnico ou dúvidas:

1. Verifique os logs do sistema
2. Consulte a documentação no README.md
3. Abra uma issue no repositório do projeto

## Atualizações

Para atualizar o sistema:

1. Faça backup dos dados importantes
2. Baixe a nova versão
3. Execute `docker-compose down`
4. Substitua os arquivos
5. Execute `docker-compose up -d --build`

O sistema manterá a compatibilidade com dados existentes através de migrações automáticas do banco de dados.


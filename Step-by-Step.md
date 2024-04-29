## Documentação Passo a Passo

### 1. Criar as Máquinas na AWS

- Utilize a interface da AWS para criar as cinco máquinas especificadas no projeto, distribuindo os sistemas operacionais e tamanhos de instância conforme necessário.

### 2. Implementação do Wazuh Manager e Agentes

## Instalar o Wazuh Manager no CONTROL (a)

A. Execute o seguinte comando na máquina a) (CONTROL):
   ```bash
   curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
   ```

B. Anote o usuário e senha fornecidos após a instalação do Wazuh.

C. No navegador, acesse com o endereço IP público da máquina a) usando HTTPS:
   ```
   https://<IP público>:443
   ```
   Faça login utilizando o usuário e senha anotados anteriormente.

D. No painel do Wazuh Manager, navegue até "Agents" e siga as instruções para adicionar os agentes nas máquinas b), c), d) e e), utilizando os seguintes comandos específicos para cada máquina.

### 2.1. Instalar Agentes Wazuh nas Máquinas Cliente

Na Máquina b) (Cliente Debian):

```bash
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.3-1_amd64.deb && \
sudo WAZUH_MANAGER='<IP_Control_Server>' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='LUX1-Debian' dpkg -i ./wazuh-agent_4.7.3-1_amd64.deb && \
```
```bash
sudo systemctl daemon-reload  
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

Na Máquina c) (Cliente RedHat):

```bash
curl -o wazuh-agent-4.7.3-1.x86_64.rpm https://packages.wazuh.com/4.x/yum/wazuh-agent-4.7.3-1.x86_64.rpm && \
sudo WAZUH_MANAGER='<IP_Control_Server>' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='LUX2-RedHat' rpm -ihv wazuh-agent-4.7.3-1.x86_64.rpm && \
```
```bash
sudo systemctl daemon-reload 
sudo systemctl enable wazuh-agent 
sudo systemctl start wazuh-agent
```

Na Máquina d) (Cliente Windows 2022):

```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.3-1.msi -OutFile ${env.tmp}\wazuh-agent; \
msiexec.exe /i ${env.tmp}\wazuh-agent /q WAZUH_MANAGER='<IP_Control_Server>' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='WIN2022' WAZUH_REGISTRATION_SERVER='<IP_Control_Server>'; \
```
```powershell
NET START WazuhSvc
```

Na Máquina e) (Cliente Windows 11):

```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.3-1.msi -OutFile ${env.tmp}\wazuh-agent; \
msiexec.exe /i ${env.tmp}\wazuh-agent /q WAZUH_MANAGER='<IP_Control_Server>' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='WIN11' WAZUH_REGISTRATION_SERVER='<IP_Control_Server>'; \
```
```powershell
NET START WazuhSvc
```

E. Após a instalação, verifique no site do Wazuh Manager na aba "Agents" se todos os agentes foram instalados corretamente.

### 3. Instalar New Relic em Todos os Agentes

A. Criação de Conta no New Relic:
   - Acesse o site do New Relic e crie uma conta para obter acesso aos recursos de monitoramento e análise oferecidos pela plataforma.
   - Após a criação da conta, gere uma nova chave API e armazene-a em um local seguro.

B. Instalação do New Relic nos Agentes:

1. Na Máquina a), b) e c):
   - Execute o seguinte comando:
     ```bash
     curl -Ls https://download.newrelic.com/install/newrelic-cli/scripts/install.sh | bash 
     sudo NEW_RELIC_API_KEY='<sua_chave>' NEW_RELIC_ACCOUNT_ID=<seu_id> NEW_RELIC_REGION=EU /usr/local/bin/newrelic install -y
     ```
   - Substitua `<sua_chave>` e `<seu_id>` pela chave API e ID da sua conta no New Relic.

2. Na Máquina d) e e):
   - Execute os seguintes comandos em PowerShell:
     ```powershell
     [Net.ServicePointManager]::SecurityProtocol = 'tls12, tls'; $WebClient = New-Object System.Net.WebClient; $WebClient.DownloadFile("https://download.newrelic.com/install/newrelic-cli/scripts/install.ps1", "$env:TEMP\install.ps1"); & PowerShell.exe -ExecutionPolicy Bypass -File $env:TEMP\install.ps1; $env:NEW_RELIC_API_KEY='<sua_chave>'; $env:NEW_RELIC_ACCOUNT_ID='<seu_id>'; $env:NEW_RELIC_REGION='EU'; & 'C:\Program Files\New Relic\New Relic CLI\newrelic.exe' install -y
     ```
   - Substitua `<sua_chave>` e `<seu_id>` pela chave API e ID da sua conta no New Relic.

C. Verificação da Configuração:
   - Acesse o site do New Relic e vá para "All Entities" para confirmar se todas as máquinas estão bem configuradas e enviando dados corretamente para o painel do New Relic.

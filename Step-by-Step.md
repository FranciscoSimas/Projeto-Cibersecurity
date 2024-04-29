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
sudo systemctl daemon-reload && \
sudo systemctl enable wazuh-agent && \
sudo systemctl start wazuh-agent
```

Na Máquina c) (Cliente RedHat):

```bash
curl -o wazuh-agent-4.7.3-1.x86_64.rpm https://packages.wazuh.com/4.x/yum/wazuh-agent-4.7.3-1.x86_64.rpm && \
sudo WAZUH_MANAGER='<IP_Control_Server>' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='LUX2-RedHat' rpm -ihv wazuh-agent-4.7.3-1.x86_64.rpm && \
```
```bash
sudo systemctl daemon-reload && \
sudo systemctl enable wazuh-agent && \
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

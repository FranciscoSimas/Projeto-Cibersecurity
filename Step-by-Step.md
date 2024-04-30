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
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.3-1_amd64.deb 
sudo WAZUH_MANAGER='<IP_Control_Server>' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='LUX1-Debian' dpkg -i ./wazuh-agent_4.7.3-1_amd64.deb 
```
```bash
sudo systemctl daemon-reload  
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

Na Máquina c) (Cliente RedHat):

```bash
curl -o wazuh-agent-4.7.3-1.x86_64.rpm https://packages.wazuh.com/4.x/yum/wazuh-agent-4.7.3-1.x86_64.rpm 
sudo WAZUH_MANAGER='<IP_Control_Server>' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='LUX2-RedHat' rpm -ihv wazuh-agent-4.7.3-1.x86_64.rpm 
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





### 4. Criar um Servidor HTTP no Debian (Máquina b))

A. Instalação e Configuração do Nginx:

1. Atualizar Listas de Pacotes: Certifique-se de que as listas de pacotes estão atualizadas executando:
   ```bash
   sudo apt update
   ```

2. Instalar o Nginx: Utilize o seguinte comando para instalar o Nginx:
   ```bash
   sudo apt install nginx
   ```

3. Iniciar o Nginx: Após a instalação, inicie o serviço do Nginx:
   ```bash
   sudo systemctl start nginx
   ```

4. Habilitar Inicialização Automática do Nginx: Para garantir que o Nginx seja iniciado automaticamente durante o boot do sistema, execute:
   ```bash
   sudo systemctl enable nginx
   ```

5. Verificar o Status do Nginx: Verifique se o Nginx está em execução executando:
   ```bash
   sudo systemctl status nginx
   ```
B. Verificação do Funcionamento do Servidor:

Acesse o seguinte link em um navegador web:
```
http://<ip_publico_da_máquina_b>
```

Certifique-se de substituir `<ip_publico_da_máquina_b>` pelo endereço IP público da máquina b. Isso permitirá que você verifique se o servidor HTTP está funcionando corretamente.




### 5. Criar um Servidor HTTPS no RedHat (Máquina c))

A. Configuração do Servidor HTTPS com Apache:

1. Instale o Apache: Se ainda não tiver instalado o Apache, faça-o executando:
   ```bash
   sudo yum install httpd
   ```

2. Instale o OpenSSL: O HTTPS depende da criptografia SSL/TLS, fornecida pelo OpenSSL. Instale-o com:
   ```bash
   sudo yum install openssl
   ```

3. Crie o diretório /etc/ssl/private/:
   ```bash
   sudo mkdir /etc/ssl/private
   ```

4. Defina permissões apropriadas para o diretório para garantir que apenas o root possa ler e escrever nele:
   ```bash
   sudo chmod 700 /etc/ssl/private
   ```

5. Agora, você pode gerar o certificado e a chave SSL:
   ```bash
   sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
   ```

6. Ative o Módulo SSL: O Apache precisa ter o módulo SSL habilitado. Você pode fazer isso executando:
   ```bash
   sudo yum install mod_ssl
   ```

7. Reinicie o Apache: Após fazer essas alterações, reinicie o Apache para aplicar a configuração:
   ```bash
   sudo systemctl restart httpd
   ```

B. Verificação do Funcionamento do Servidor:

Acesse o seguinte link em um navegador web:
```
https://<ip_publico_da_máquina_c>
```

Certifique-se de substituir `<ip_publico_da_máquina_c>` pelo endereço IP público da máquina c. Isso permitirá que você verifique se o servidor HTTPS está funcionando corretamente.





### 6. Criar um Servidor HTTP e HTTPS no Windows (Máquina d)

A. Configuração do Servidor HTTP e HTTPS com IIS:

1. Abrir o Server Manager:
   - Acesse o Server Manager na máquina Windows Server (Máquina d).

2. Adicionar Papéis e Recursos:
   - No Server Manager, clique em "Add roles and features".
   - Avance até a seção "Server Roles" e selecione a opção "Web Server (IIS)".
   - Siga as instruções para concluir a instalação.

3. Acessar o IIS Manager:
   - No Server Manager, vá para "Tools" e entre no IIS Manager.

4. Gerar Certificado Autoassinado:
   - No IIS Manager, clique no servidor localhost e vá para "Server Certificates".
   - Crie um "Self-Signed Certificate", selecionando a opção "WebHosting" e dando um nome personalizado.

5. Configurar Bindings do Site Padrão:
   - No IIS Manager, vá para os sites e acesse "Default Web Site".
   - Configure as bindings do site padrão adicionando HTTPS e selecionando o certificado recém-criado.

B. Verificação do Funcionamento do Servidor:

1. Acesso ao Servidor HTTP:
   - No navegador, acesse o seguinte link:
     ```
     http://<ip_publico_da_máquina_d>
     ```
   Certifique-se de substituir `<ip_publico_da_máquina_d>` pelo endereço IP público da máquina d). Isso permitirá que você verifique se o servidor HTTP está funcionando corretamente.

2. Acesso ao Servidor HTTPS:
   - No navegador, acesse o seguinte link:
     ```
     https://<ip_publico_da_máquina_d>
     ```
   Certifique-se de substituir `<ip_publico_da_máquina_d>` pelo endereço IP público da máquina d). Isso permitirá que você verifique se o servidor HTTPS está funcionando corretamente.





### 7. Configurar os Servidores para o UptimeRobot

A. Criação de Conta:
   - Acesse o site do UptimeRobot e crie uma conta para obter acesso aos recursos de monitoramento oferecidos pela plataforma.

B. Configuração dos Monitores:

1. Servidor Nginx na Máquina b) (Porta 80):
   - Crie um novo monitor no UptimeRobot utilizando o seguinte URL:
     ```
     http://<ip_publico_da_máquina_b>
     ```

2. Servidor IIS na Máquina d) (Porta 80):
   - Crie um novo monitor no UptimeRobot utilizando o seguinte URL:
     ```
     http://<ip_publico_da_máquina_d>
     ```

3. Servidor IIS na Máquina d) (Porta 443 - HTTPS):
   - Crie um novo monitor no UptimeRobot utilizando o seguinte URL:
     ```
     https://<ip_publico_da_máquina_d>
     ```

4. Servidor Apache na Máquina c) (Porta 443 - HTTPS):
   - Crie um novo monitor no UptimeRobot utilizando o seguinte URL:
     ```
     https://<ip_publico_da_máquina_c>
     ```

Certifique-se de substituir `<ip_publico_da_máquina>` pelo endereço IP público correspondente de cada máquina. Isso permitirá que o UptimeRobot monitore o status de cada servidor e porta especificados.





### 8. Integrar o PagerDuty no Wazuh

A. Criação de Conta no PagerDuty:
   - Acesse o site do PagerDuty e crie uma conta para obter acesso aos recursos de gerenciamento de incidentes oferecidos pela plataforma.

B. Configuração do Serviço no PagerDuty:

1. Acesse o PagerDuty e vá para "Services".
2. Clique em "New Service" e dê um nome ao serviço.
3. Continue a instalação até chegar à opção "Events API V2".
4. Guarde a API_KEY fornecida durante a configuração do serviço.

C. Configuração no Wazuh (Máquina CONTROL):

1. Acesse a máquina CONTROL e execute o comando:
   ```bash
   sudo su
   ```

2. Edite o arquivo de configuração do Wazuh:
   ```bash
   nano /var/ossec/etc/ossec.conf
   ```

3. Integre o PagerDuty no arquivo de configuração adicionando o seguinte trecho:
   ```xml
   <integration>
     <name>pagerduty</name>
     <api_key><API_KEY></api_key> <!-- Substitua pela sua chave de API do PagerDuty -->
     <level>10</level>
     <alert_format>json</alert_format> <!-- Novo parâmetro obrigatório desde v4.7.0 -->
   </integration>
   ```
   Substitua `<API_KEY>` pela chave API do PagerDuty que você salvou anteriormente.

4. Salve o arquivo de configuração.

D. Reinicie o Serviço do Wazuh:
   Execute o seguinte comando para reiniciar o serviço:
   ```bash
   systemctl restart wazuh-manager
   ```

E. Verificação da Integração com o PagerDuty:
   - Acesse o PagerDuty, vá para "Services" e confirme se o Wazuh foi corretamente configurado como um serviço.

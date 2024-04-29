# Projeto-Cibersecurity
# Documentação do Projeto de Segurança Cyber-Security

## 1. Configuração de Infraestrutura na AWS

### Máquinas na AWS:
1. **Control Server (Windows 11) - a)**
   - Tipo: c5.large
   - Acesso: RDP (Remmina)

2. **Cliente Debian - b)**
   - Tipo: t2.micro
   - Acesso: Termius (SSH)

3. **Cliente RedHat - c)**
   - Tipo: t2.micro
   - Acesso: Termius (SSH)

4. **Cliente Windows 2022 - d)**
   - Tipo: c5.large
   - Acesso: RDP (Remmina)

5. **Cliente Windows 11 - e)**
   - Tipo: c5.large
   - Acesso: RDP (Remmina)

### Configuração do Security Group:
- Todas as máquinas compartilham o mesmo security group.
- Acesso público via IP.
- Inbound:


- Outbound:


## 2. Instalação e Configuração de Aplicações Principais

### No Control Server:
- **Aplicações Servidor:**
  1. New Relic
  2. Wazuh
  3. Uptime Robot
  4. Pagerduty
  5. Splunk

### Nos Clientes (Agentes):
- **Aplicações Cliente:**
  - Instalar e configurar os seguintes clientes/agentes para se conectarem ao servidor Control:
    1. New Relic
    2. Wazuh
    3. Uptime Robot
    4. Pagerduty
    5. Splunk

## 2.1 Instalação e Configuração Adicional

### Na Máquina b) (Cliente Debian):
- **Servidor HTTP:**
  - Instalar NGINX (ou outro servidor HTTP) e configurá-lo.

### Na Máquina c) (Cliente RedHat):
- **Servidor HTTPS:**
  - Instalar Apache2 (ou outro servidor HTTPS) e configurá-lo.

### Na Máquina d) (Cliente Windows 2022):
- **Servidor HTTP e HTTPS:**
  - Instalar IIS através do Server Manager do Windows e configurar HTTP e HTTPS.

## 3. Configuração dos Agentes de Segurança

- Todos os Clientes (b, c, d, e) devem ter os agentes de New Relic, Wazuh, Uptime Robot, Pagerduty e Splunk instalados e configurados.
- O objetivo é manter todos os servidores, aplicações e serviços com máxima segurança, visando alcançar 100% de segurança em todas as máquinas. 

# Cybersecurity Project
# Cyber-Security Project Documentation

## 1. Infrastructure Setup in AWS

### AWS Machines:
1. **Control Server (Ubuntu) - a)**
   - Type: c5.large
   - Access: Termius (SSH)

2. **Debian Client - b)**
   - Type: t2.micro
   - Access: Termius (SSH)

3. **RedHat Client - c)**
   - Type: t2.micro
   - Access: Termius (SSH)

4. **Windows 2022 Client - d)**
   - Type: c5.large
   - Access: RDP (Remmina)

5. **Windows 11 Client - e)**
   - Type: c5.large
   - Access: RDP (Remmina)

### Security Group Configuration:
- All machines share the same security group.
- Public access via IP.
- Inbound:


- Outbound:


## 2. Installation and Configuration of Main Applications

### On Control Server:
- **Server Applications:**
  1. New Relic
  2. Wazuh
  3. Uptime Robot
  4. PagerDuty
  5. Splunk

### On Clients (Agents):
- **Client Applications:**
  - Install and configure the following clients/agents to connect to the Control server:
    1. New Relic
    2. Wazuh
    3. Uptime Robot
    4. PagerDuty
    5. Splunk

## 2.1 Additional Installation and Configuration

### On Machine b) (Debian Client):
- **HTTP Server:**
  - Install NGINX (or another HTTP server) and configure it.

### On Machine c) (RedHat Client):
- **HTTPS Server:**
  - Install Apache2 (or another HTTPS server) and configure it.

### On Machine d) (Windows 2022 Client):
- **HTTP and HTTPS Server:**
  - Install IIS through Windows Server Manager and configure HTTP and HTTPS.

## 3. Security Agents Configuration

- All Clients (b, c, d, e) must have New Relic, Wazuh, Uptime Robot, PagerDuty, and Splunk agents installed and configured.
- The goal is to maintain all servers, applications, and services with maximum security, aiming to achieve 100% security on all machines.

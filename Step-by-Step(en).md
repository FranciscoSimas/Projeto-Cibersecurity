## Step-by-Step Documentation

### 1. Creating Machines on AWS

- Use the AWS interface to create the five machines specified in the project, distributing the operating systems and instance sizes as needed.

### 2. Wazuh Manager and Agents Implementation

## Install Wazuh Manager on CONTROL (a)

A. Run the following command on machine a) (CONTROL):
   ```bash
   curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
   ```

B. Note the username and password provided after Wazuh installation.

C. In the browser, access using the public IP address of machine a) via HTTPS:
   ```
   https://<public_IP>:443
   ```
   Log in using the username and password noted earlier.

D. In the Wazuh Manager panel, navigate to "Agents" and follow the instructions to add agents on machines b), c), d), and e), using the following specific commands for each machine.

### 2.1. Install Wazuh Agents on Client Machines

On Machine b) (Debian Client):

```bash
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.3-1_amd64.deb 
sudo WAZUH_MANAGER='<IP_Control_Server>' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='LUX1-Debian' dpkg -i ./wazuh-agent_4.7.3-1_amd64.deb 
```
```bash
sudo systemctl daemon-reload  
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

On Machine c) (RedHat Client):

```bash
curl -o wazuh-agent-4.7.3-1.x86_64.rpm https://packages.wazuh.com/4.x/yum/wazuh-agent-4.7.3-1.x86_64.rpm 
sudo WAZUH_MANAGER='<IP_Control_Server>' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='LUX2-RedHat' rpm -ihv wazuh-agent-4.7.3-1.x86_64.rpm 
```
```bash
sudo systemctl daemon-reload 
sudo systemctl enable wazuh-agent 
sudo systemctl start wazuh-agent
```

On Machine d) (Windows 2022 Client):

```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.3-1.msi -OutFile ${env.tmp}\wazuh-agent; \
msiexec.exe /i ${env.tmp}\wazuh-agent /q WAZUH_MANAGER='<IP_Control_Server>' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='WIN2022' WAZUH_REGISTRATION_SERVER='<IP_Control_Server>'; \
```
```powershell
NET START WazuhSvc
```

On Machine e) (Windows 11 Client):

```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.3-1.msi -OutFile ${env.tmp}\wazuh-agent; \
msiexec.exe /i ${env.tmp}\wazuh-agent /q WAZUH_MANAGER='<IP_Control_Server>' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='WIN11' WAZUH_REGISTRATION_SERVER='<IP_Control_Server>'; \
```
```powershell
NET START WazuhSvc
```

E. After installation, check the Wazuh Manager site on the "Agents" tab to ensure all agents were installed correctly.

### 3. Installing New Relic on All Agents

A. Creating an Account on New Relic:
   - Access the New Relic website and create an account to gain access to the monitoring and analysis resources offered by the platform.
   - After creating the account, generate a new API key and store it securely.

B. Installing New Relic on Agents:

1. On Machine a), b), and c):
   - Execute the following command:
     ```bash
     curl -Ls https://download.newrelic.com/install/newrelic-cli/scripts/install.sh | bash 
     sudo NEW_RELIC_API_KEY='<your_key>' NEW_RELIC_ACCOUNT_ID=<your_id> NEW_RELIC_REGION=EU /usr/local/bin/newrelic install -y
     ```
   - Replace `<your_key>` and `<your_id>` with your New Relic API key and account ID.

2. On Machine d) and e):
   - Execute the following commands in PowerShell:
     ```powershell
     [Net.ServicePointManager]::SecurityProtocol = 'tls12, tls'; $WebClient = New-Object System.Net.WebClient; $WebClient.DownloadFile("https://download.newrelic.com/install/newrelic-cli/scripts/install.ps1", "$env:TEMP\install.ps1"); & PowerShell.exe -ExecutionPolicy Bypass -File $env:TEMP\install.ps1; $env:NEW_RELIC_API_KEY='<your_key>'; $env:NEW_RELIC_ACCOUNT_ID='<your_id>'; $env:NEW_RELIC_REGION='EU'; & 'C:\Program Files\New Relic\New Relic CLI\newrelic.exe' install -y
     ```
   - Replace `<your_key>` and `<your_id>` with your New Relic API key and account ID.

C. Configuration Verification:
   - Access the New Relic website and go to "All Entities" to confirm if all machines are properly configured and sending data correctly to the New Relic dashboard.

### 4. Creating an HTTP Server on Debian (Machine b))

A. Installation and Configuration of Nginx:

1. Update Package Lists: Make sure package lists are up to date by running:
   ```bash
   sudo apt update
   ```

2. Install Nginx: Use the following command to install Nginx:
   ```bash
   sudo apt install nginx
   ```

3. Start Nginx: After installation, start the Nginx service:
   ```bash
   sudo systemctl start nginx
   ```

4. Enable Automatic Start of Nginx: To ensure Nginx starts automatically during system boot, execute:
   ```bash
   sudo systemctl enable nginx
   ```

5. Check Nginx Status: Verify if Nginx is running by executing:
   ```bash
   sudo systemctl status nginx
   ```

B. Server Functionality Verification:

Access the following link in a web browser:
```
http://<public_IP_of_machine_b>
```

Make sure to replace `<public_IP_of_machine_b>` with the public IP address of machine b. This will allow you to verify if the HTTP server is functioning correctly.

### 5. Creating an HTTPS Server on RedHat (Machine c))

A. HTTPS Server Configuration with Apache:

1. Install Apache: If Apache is not already installed, do so by running:
   ```bash
   sudo yum install httpd
   ```

2. Install OpenSSL: HTTPS relies on SSL/TLS encryption, provided by OpenSSL. Install it with:
   ```bash
   sudo yum install openssl
   ```

3. Create the /etc/ssl/private/ directory:
   ```bash
   sudo mkdir /etc/ssl/private
   ```

4. Set appropriate permissions for the directory to ensure only root can read and write to it:
   ```bash
   sudo chmod 700 /etc/ssl/private
   ```

5. Now, you can generate the SSL certificate and key:
   ```bash
   sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
   ```

6. Enable the SSL Module: Apache needs to have the SSL module enabled. You can do this by running:
   ```bash
   sudo yum install mod_ssl
   ```

7. Restart Apache: After making these changes, restart Apache to apply the configuration:
   ```bash
   sudo systemctl restart httpd
   ```

B. Server Functionality Verification:

Access the following link in a web browser:
```
https://<public_IP_of_machine_c>
```

Make sure to replace `<public_IP_of_machine_c>` with the public IP address of machine c. This will allow you to verify if the HTTPS server is functioning correctly.

### 6. Creating HTTP and HTTPS Servers on Windows (Machine d)

A. HTTP and HTTPS Server Configuration with IIS:

1. Open Server Manager:
   - Access Server Manager on the Windows Server machine (Machine d).

2. Add Roles and Features:
   - In Server Manager, click on "Add roles and features".
   - Proceed to the "Server Roles" section and select "Web Server (IIS)".
   - Follow the instructions to complete the installation.

3. Access the IIS Manager:
   - In Server Manager, go to "Tools" and enter the IIS Manager.

4. Generate Self-Signed Certificate:
   - In the IIS Manager, click on the localhost server and go to "Server Certificates".
   - Create a "Self-Signed Certificate", selecting the "WebHosting" option and giving it a custom name.

5. Configure Default Site Bindings:
   - In the IIS Manager, go to sites and access "Default Web Site".
   - Configure the bindings of the default site by adding HTTPS and selecting the newly created certificate.

B. Server Functionality Verification:

1. Accessing the HTTP Server:
   - In the browser, access the following link:
     ```
     http://<public_IP_of_machine_d>
     ```
   Make sure to replace `<public_IP_of_machine_d>` with the public IP address of machine d. This will allow you to verify if the HTTP server is functioning correctly.

2. Accessing the HTTPS Server:
   - In the browser, access the following link:
     ```
     https://<public_IP_of_machine_d>
     ```
   Make sure to replace `<public_IP_of_machine_d>` with the public IP address of machine d. This will allow you to verify if the HTTPS server is functioning correctly.

### 7. Configuring Servers for UptimeRobot

A. Account Creation:
   - Access the UptimeRobot website and create an account to gain access to the monitoring resources offered by the platform.

B. Monitor Configuration:

1. Nginx Server on Machine b) (Port 80):
   - Create a new monitor in UptimeRobot using the following URL:
     ```
     http://<public_IP_of_machine_b>
     ```

2. IIS Server on Machine d) (Port 80):
   - Create a new monitor in UptimeRobot using the following URL:
     ```
     http://<public_IP_of_machine_d>
     ```

3. IIS Server on Machine d) (Port 443 - HTTPS):
   - Create a new monitor in UptimeRobot using the following URL:
     ```
     https://<public_IP_of_machine_d>
     ```

4. Apache Server on Machine c) (Port 443 - HTTPS):
   - Create a new monitor in UptimeRobot using the following URL:
     ```
     https://<public_IP_of_machine_c>
     ```

Make sure to replace `<public_IP_of_machine>` with the corresponding public IP address of each machine. This will allow UptimeRobot to monitor the status of each specified server and port.

### 8. Integrating PagerDuty with Wazuh

A. PagerDuty Account Creation:
   - Access the PagerDuty website and create an account to gain access to the incident management resources offered by the platform.

B. Service Configuration in PagerDuty:

1. Access PagerDuty and go to "Services".
2. Click on "New Service" and give the service a name.
3. Continue the setup until you reach the "Events API V2" option.
4. Save the API_KEY provided during the service configuration.

C. Configuration in Wazuh (Machine CONTROL):

1. Access the CONTROL machine and execute the command:
   ```bash
   sudo su
   ```

2. Edit the Wazuh configuration file:
   ```bash
   nano /var/ossec/etc/ossec.conf
   ```

3. Integrate PagerDuty into the configuration file by adding the following snippet:
   ```xml
   <integration>
     <name>pagerduty</name>
     <api_key><API_KEY></api_key> <!-- Replace with your PagerDuty API key -->
     <level>5</level>
     <alert_format>json</alert_format> <!-- New mandatory parameter since v4.7.0 -->
   </integration>
   ```
   Replace `<API_KEY>` with the PagerDuty API key you saved earlier.

4. Save the configuration file.

D. Restart Wazuh Service:
   Execute the following command to restart the service:
   ```bash
   systemctl restart wazuh-manager
   ```

E. Verification of Integration with PagerDuty:
   - Access PagerDuty, go to "Services", and confirm if Wazuh has been correctly configured as a service.

### 9. Splunk Installation on CONTROL Machine

A. Splunk Installation:

1. Access the Splunk website and get the download link for "Splunk Enterprise".
2. On the CONTROL machine, download the Splunk package using the wget command and install it with the following command:
   ```bash
   dpkg -i splunk-9.2.1-78803f08aabb-linux-2.6-amd64.deb
   ```
3. Start the Splunk service:
   ```bash
   sudo /opt/splunk/bin/splunk start
   ```
4. Configure Splunk to start automatically during system boot:
   ```bash
   sudo /opt/splunk/bin/splunk enable boot-start
   ```

B. Server Functionality Verification:

Access the following link in a web browser:
```
http://<public_IP_of_machine_CONTROL>:8000
```
Make sure to replace `<public_IP_of_machine_CONTROL>` with the public IP address of the CONTROL machine. This will allow you to verify if the Splunk server is functioning correctly.

### 10. Project Conclusion

With all services installed and configured as described in this project, it's time to delve deeper into each service's functionalities and explore the possibilities of integration between them. Some suggestions for further advancement:

1. **Explore Splunk Features:**
   - Spend time exploring Splunk's features such as creating dashboards, running advanced queries, setting up alerts, and more. Splunk is a powerful data analysis platform that can provide valuable insights into the security environment.

2. **Enhancements in Wazuh:**
   - Dive into Wazuh settings to reduce vulnerabilities by adjusting detection rules, configuring stricter security policies, and implementing automatic corrective actions.

3. **Additional Integrations:**
   - Consider integrating other security services or monitoring tools into your environment, such as additional intrusion detection systems, intrusion prevention systems, log analysis solutions, among others.

4. **Training and Certifications:**
   - Consider participating in specific training courses for the technologies used in this project and seek relevant certifications, which can help enhance your skills and knowledge.

5. **Baseline Implementation:** Establish normal behavior baselines for systems and applications in the environment, making it easier to detect deviations and suspicious behaviors that may indicate malicious activities.

6. **Audits and Periodic Reviews:**
   - Conduct periodic security audits in your environment, reviewing configurations, identifying potential security loopholes, and implementing corrective measures as necessary.

By continuing to explore and enhance your cybersecurity project, you'll be strengthening the protection of the environment and contributing to the security and integrity of data and systems.

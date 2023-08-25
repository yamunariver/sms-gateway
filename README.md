# sms-gateway


Setting up Kannel for SMS on Ubuntu 20.04 involves a series of steps, including installing the necessary software, configuring Kannel, and testing the SMS functionality. Here's a detailed guide to help you through the process:

**Step 1: Install Required Packages**
Open a terminal and run the following commands to install the required packages:

```bash
sudo apt update
sudo apt install kannel mysql-server
```

**Step 2: Configure Kannel**
1. Create a configuration directory for Kannel:

```bash
sudo mkdir /etc/kannel
```

2. Navigate to the configuration directory:

```bash
cd /etc/kannel
```

3. Create a Kannel configuration file named `kannel.conf`:

```bash
sudo nano kannel.conf
```

4. Populate `kannel.conf` with the following basic configuration. Replace the values with your actual settings:

```plaintext
group = core
admin-port = 13000
smsbox-port = 13001
admin-password = your_admin_password

group = smsc
smsc = fake
smsc-id = fake
port = 10001

group = smsbox
bearerbox-host = localhost
sendsms-port = 13013
log-file = "/var/log/kannel/smsbox.log"
access-log = "/var/log/kannel/access.log"
log-level = 0
```

5. Save and close the file.

**Step 3: Create Log Directories**
Create the necessary log directories:

```bash
sudo mkdir /var/log/kannel
sudo chown kannel:kannel /var/log/kannel
```

**Step 4: Start Kannel**
Start Kannel's components:

```bash
sudo systemctl start kannel
sudo systemctl enable kannel
```

**Step 5: Test SMS Sending**
1. Open a terminal and use the `sendsms` command to send a test SMS:

```bash
sendsms HTTP --method POST "http://127.0.0.1:13013/cgi-bin/sendsms" \
  username="your_username" password="your_password" \
  to="recipient_number" text="Test SMS from Kannel"
```

Replace `your_username`, `your_password`, and `recipient_number` with appropriate values.

2. Check the logs for any errors:

```bash
sudo tail -f /var/log/kannel/smsbox.log
```

**Step 6: Set Up MySQL Database (Optional)**
If you plan to use a MySQL database for storing SMS-related data, you need to set it up and configure Kannel accordingly. This involves creating a database, a user, and granting necessary privileges. Then, modify the Kannel configuration file to use MySQL as the storage backend.

Please note that this step can be quite complex and depends on your specific use case. If you decide to use MySQL, refer to the Kannel documentation for detailed instructions on setting up the database backend.

Keep in mind that this is a basic guide to get you started with Kannel on Ubuntu 20.04. For production environments and more complex setups, additional considerations, security measures, and configurations may be necessary. Always consult the official Kannel documentation for the most up-to-date and comprehensive information.



#Tsteps

Certainly, troubleshooting Kannel can involve various aspects, including configuration issues, connectivity problems, and service failures. Here are some detailed troubleshooting steps to help you address common problems:

**1. Verify Kannel Service Status:**
Check if the Kannel service is running:

```bash
sudo systemctl status kannel
```

If the service is not active, try starting it:

```bash
sudo systemctl start kannel
```

**2. Check Log Files:**
Kannel logs contain valuable information about errors and issues. Common log files to check include:

- SMSBOX Log: `/var/log/kannel/smsbox.log`
- Bearerbox Log: `/var/log/kannel/bearerbox.log`
- Access Log: `/var/log/kannel/access.log`

Use the `tail` command to view the last few lines of a log file:

```bash
sudo tail -f /var/log/kannel/smsbox.log
```

**3. Validate Configuration:**
Ensure that your `kannel.conf` file is properly configured. Pay attention to the syntax and values of various parameters. Typos and incorrect configurations can cause problems.

You can validate the configuration file using:

```bash
sudo kannel -dddddddd -config /etc/kannel/kannel.conf
```

**4. Test SMS Sending:**
Use the `sendsms` command-line tool to send a test SMS through Kannel. This helps identify if the SMS functionality is working:

```bash
sendsms HTTP --method POST "http://127.0.0.1:13013/cgi-bin/sendsms" \
  username="your_username" password="your_password" \
  to="recipient_number" text="Test SMS from Kannel"
```

**5. Check Network Connectivity:**
Ensure that Kannel's components are correctly configured to listen on the appropriate ports. Use tools like `netstat` or `ss` to check port availability:

```bash
netstat -tuln | grep "LISTEN"
```

**6. Firewall Configuration:**
Check if the firewall is blocking incoming/outgoing connections on the required ports (e.g., 13013, 13001).

**7. MySQL Database Connection:**
If you're using MySQL, verify that Kannel can connect to the database using the configured credentials. Check if the MySQL server is running and accessible.

**8. Ownership and Permissions:**
Ensure that Kannel's log directories and configuration files have the correct ownership and permissions. Use `chown` and `chmod` to set them appropriately.

**9. Memory and Resources:**
Monitor system resources (memory, CPU, disk space) to ensure that Kannel is not being restricted due to lack of resources.

**10. Check Dependencies:**
Make sure that all required dependencies are installed and correctly configured. These can include libraries, packages, and other software that Kannel relies on.

**11. Test Different SMS Centers:**
If you're using multiple SMS centers, test sending SMS through different centers to identify if the issue is specific to one.

**12. Read Documentation and Forums:**
Search Kannel's official documentation and online forums for solutions to common problems. Other users' experiences and solutions can be quite helpful.

**13. Debug Mode:**
Use debug mode (`-dddddddd`) while starting Kannel to get detailed debugging information, which can help identify the root cause of the problem.

Remember that troubleshooting can be a step-by-step process. Start by verifying basic elements like service status and configuration, and gradually narrow down the potential issues. If you're unable to resolve a problem, consulting the Kannel community or seeking professional support might be necessary.

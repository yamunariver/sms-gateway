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

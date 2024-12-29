# Frappe Installation

## Create seperate user profile for frappe in WSL

```bash
sudo adduser [user profile]
```
### modeify the User profile to sudo
```bash
usermod -aG sudo [user profile]
```
### Make or Login to  User profile as Super user
```bash
su [user profile]
```
### CD to the user profile
```bash
cd /home/[user profile]/
```

### Prerequisites to Install Frappe in WSL

### Step 1: Update the Packages
Run the following command to update and upgrade your packages:

```bash
sudo apt-get update -y
```
### Step 2: Upgrade Packages

Run the following command to upgrade your installed packages:

```bash
sudo apt-get upgrade -y
```

### Step 3: Install Git
```bash
sudo apt-get install git
```
### Step 4: Install python Dev
```bash
sudo apt-get install python3-dev -y
```
### Step 5: Install setuptools and Pip
```bash
sudo apt-get install python3-setuptools python3-pip -y
```
### Step 6: create Virtual Environment
```bash
sudo apt install python3.12-venv -y
```
### Install maria DB
```bash
sudo apt-get install software-properties-common -y
```
```bash
sudo apt install mariadb-server -y
```
```bash
sudo systemctl status mariadb
```
```bash
sudo mysql_secure_installation
```
### Do following configuration
When you run this command, the server will show the following prompts. Please follow the steps as shown below to complete the setup correctly.
* Enter current password for root: (Enter your SSH root user password)
  Switch to unix_socket authentication [Y/n]: Y
  Change the root password? [Y/n]: Y
* It will ask you to set new MySQL root password at this step. This can be different from the SSH root user password.
  Remove anonymous users? [Y/n] Y
  Disallow root login remotely? [Y/n]: N
* This is set as N because we might want to access the database from a remote server for using business analytics software like Metabase / PowerBI / Tableau, etc.
  Remove test database and access to it? [Y/n]: Y
  Reload privilege tables now? [Y/n]: Y

  ### Edit mysql default Config file
  ```bash
  sudo nano /etc/mysql/my.cnf
  ```
### Add the following block of code to the end of the file, exactly as is:
```bash
[mysqld] 
character-set-client-handshake = FALSE 
character-set-server = utf8mb4 
collation-server = utf8mb4_unicode_ci 
[mysql] 
default-character-set = utf8mb4
```

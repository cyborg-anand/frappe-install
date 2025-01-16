start# Frappe Installation in windows 10/11- WSL through Frappe Bench

## Create seperate user profile for frappe in WSL
```bash
sudo adduser [user profile]
```
modify the User profile to sudo
```bash
sudo usermod -aG sudo [user profile]
```
Make or Login to  User profile as Super user
```bash
su [user profile]
```
CD to the user profile
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
* Switch to unix_socket authentication [Y/n]: Y
* Change the root password? [Y/n]: Y
* It will ask you to set new MySQL root password at this step. This can be different from the 
  SSH root user password.
* Remove anonymous users? [Y/n] Y
* Disallow root login remotely? [Y/n]: N
* This is set as N because we might want to access the database from a remote server for 
  using business analytics software like Metabase / PowerBI / Tableau, etc.
* Remove test database and access to it? [Y/n]: Y
* Reload privilege tables now? [Y/n]: Y

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
### Restart the server
```bash
sudo service mysql restart
```
### Install redis Server
```bash
sudo apt-get install redis-server -y
```
## Instal CURL, Node, NPM and Yarn
### Install CURL
```bash
sudo apt install curl
```
### Install node (As of now Node version 18  works good)
```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.bashrc
nvm install 18
```
### Install NPM
```bash
sudo apt-get install npm -y
```
### Install Yarn
```bash
sudo npm install -g yarn -y
```
### Install wkhtmltopdf
```bash
sudo apt-get install xvfb libfontconfig wkhtmltopdf -y
```
1. Fix Missing Packages
a. Install libjpeg62-turbo Alternative
The libjpeg62-turbo package might have been replaced or is unavailable for your current Ubuntu version. Try installing the libjpeg8 or libjpeg-turbo8 package, which should be compatible:

```bash
sudo apt-get install libjpeg8-dev
```
b. Install libssl1.1 Alternative
If libssl1.1 is missing, it’s possible that your version of Ubuntu no longer supports it. You can install libssl1.0.0 instead (though this might depend on your specific system):

```bash
sudo apt-get install libssl1.0.0
```
Alternatively, you can try installing libssl-dev which should include SSL libraries for development:
```bash
sudo apt-get install libssl-dev
```
2. Update Your Package Index
Sometimes, package index issues can cause these errors. Update your package lists to ensure your repositories are up-to-date:

```bash
sudo apt-get update
```
3. Install the Packages Again
After fixing these issues, you can reattempt installing the necessary dependencies:

```bash
sudo apt-get install -y \
  wget \
  build-essential \
  libssl-dev \
  libfontconfig1 \
  libjpeg8-dev \
  libx11-dev \
  libxrender-dev \
  libxext-dev \
  qtbase5-dev \
  qtchooser \
  qt5-qmake \
  qtbase5-dev-tools \
  libpng-dev
```
### Then Download the packagge
Step 1:
```bash
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.jammy_amd64.deb
```
Step 2:
```bash
sudo dpkg -i wkhtmltox_0.12.6.1-2.jammy_amd64.deb
```
Check installation. 
```bash
wkhtmltopdf --version
```
The output should be something like this wkhtmltopdf 0.12.x.x (with patched qt)

### Install frappe bench
```bash
sudo -H pip3 install frappe-bench --break-system-packages
```
### Install Ansible
```bash
sudo -H pip3 install ansible --break-system-packages
```
### Initialize Frappe Bench
```bash
bench init {bench-name} --frappe-branch version-15
```
### Switch directories into the Frappe Bench directory
```bash
cd {bench-name}
```
### Change user directory permissions
This will give the bench user execution permission to the home directory.
```bash
chmod -R o+rx /home/[user profile]
```
### Start The Bench
```bash
bench start
```
### Create a New Site
```bash
bench new-site [site-name]
```
Enter the MySQL root password which we created while installing mariaDb
Next It will ask to Set Administrator password for the site we created, You can set a password to login to the site and Reenter the password again, 
### This step is Optional
```bash  
bench --site [site-name] add-to-hosts
```
## Install ERPNext and other Apps in Frappe Bench
### Download all the apps we want to install
```bash
bench get-app erpnext --branch version-15
```
### If any other apps we can install like this 
```bash
bench get-app hrms
```
### Install the required app in the site
```bash
bench --site [site-name] install-app erpnext
```
```bash
bench --site [site-name] install-app hrms
```
To enable developer mode do it immediately after installing app to sites before configuration in the UI
```bash
bench set-config -g developer_mode 1
```
alternativelyy we can do for particulaar site only by this cmd, 
[ bench --site your-site-name set-config developer_mode 1 ]

### If bench is running stop it and again start
```bash
bench start
```
Open url http://[site-name]:8000 to login


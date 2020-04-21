# Installation

## Installation

> This guide is designed for **new Ubuntu 18.04** servers. If you are not using **Ubuntu 18.04**, you may need to take extra steps to complete the early installation process. Follow these steps **in order!**

1. [System setup](https://github.com/SoapboxRaceWorld/soapbox-race-core/wiki/Installation#part-1-system-setup)
2. [Code compilation](https://github.com/SoapboxRaceWorld/soapbox-race-core/wiki/Installation#part-2-code-compilation)
3. [Software configuration: Openfire](https://github.com/SoapboxRaceWorld/soapbox-race-core/wiki/Installation#part-3-software-configuration-openfire)
4. TODO

###  Part 1: System setup

####  Account

Create a **non-root** account, and give it **sudo** permissions. This can be done with 2 commands \(executed as `root`\):

```text
adduser <user name>
usermod -aG sudo <user name>
```

where `<user name>` is the name of the new account. Give the account a strong password, and make sure you can log in to it through SSH. The rest of this guide must be followed as the new account.

####  MySQL

After adding the new account, MySQL must be installed. MySQL 8 is strongly recommended. You can install MySQL with these commands:

```text
curl -OL https://dev.mysql.com/get/mysql-apt-config_0.8.14-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.14-1_all.deb
(a menu will appear, select OK and press ENTER)
sudo apt update -y && sudo apt install -y mysql-server
```

After running the last commands, you will be asked for a **root password**. Although you will never use the root account after you set up your server, you should still set a secure password for it. Once MySQL is fully configured, run this command: `mysql -uroot -p` When you are prompted for a password, enter the root password you set before and press ENTER.

You will now create MySQL accounts. Generate 3 strong passwords. One will be the MANAGERPASS, the next will be the GAMEPASS and the last will be the CHATPASS.

Type each of these commands, pressing ENTER after each one:

```text
CREATE USER manager@'%' IDENTIFIED BY "MANAGERPASS";
CREATE USER game@localhost IDENTIFIED BY "GAMEPASS";
CREATE USER openfire@localhost IDENTIFIED BY "CHATPASS";

CREATE DATABASE SOAPBOX;
CREATE DATABASE openfire;

GRANT ALL ON SOAPBOX.* to manager@'%', game@localhost;
GRANT ALL ON openfire.* to manager@'%', openfire@localhost;
exit;
```

####  Tools

To compile and run the server, you will need to install some tools. Run the following commands:

```text
sudo apt install -y zip unzip
sudo apt install -y redis-server
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk install java
sdk install maven
```

This will install the latest version of Java \(the platform that the server runs on\), Maven \(the tool that is used to create the server .jar file\) and Redis \(software that is used to keep track of various **temporary** data\)

###  Part 2: Code compilation

To acquire the code required to run the server, run the following commands:

```text
mkdir -p ~/server/src
mkdir -p ~/server/deploy

git clone https://github.com/SoapboxRaceWorld/soapbox-race-core.git ~/server/src/core
git clone https://github.com/SoapboxRaceWorld/openfire.git ~/server/src/openfire
git clone https://github.com/SoapboxRaceWorld/openfire-restAPI-plugin.git ~/server/src/openfire-restAPI-plugin
git clone https://github.com/SoapboxRaceWorld/openfire-nonSaslAuthentication-plugin.git ~/server/src/openfire-nonSaslAuthentication-plugin
```

Now you need to compile the code. To do that, run the following commands:

```text
cd ~/server/src/core
mvn package -Dnfs.core.stage=production
cd ~/server/src/openfire
mvn verify
cd ~/server/src/openfire-restAPI-plugin
mvn verify
cd ~/server/src/openfire-nonSaslAuthentication-plugin
mvn verify
```

_TODO_

###  Part 3: Software configuration: Openfire

_TODO_


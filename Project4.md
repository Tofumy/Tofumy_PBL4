# Project 4

**Step 1** - Install NodeJs
---

The below cmdlet update Ubuntu

`sudo apt update`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-update.png)


Upgraded ubuntu

`sudo apt upgrade`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-upgrade.png)

Installed NodeJS

`sudo apt install -y nodejs`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-apt-install-mongodb.png)

**Step 2** : Install MongoDB

`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`


`echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-apt-key-adv.png)



To install the MongoDB, we run the below:

``

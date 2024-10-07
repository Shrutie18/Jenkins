# Jenkins
1) Create two  ubuntu instances, one for jenkins and another for Gitea.
   ![image](https://github.com/user-attachments/assets/f807315d-f1ab-40ac-aeb0-21be69c21320)
   make sure you add these ports for  launching jenkins (port 8080 must for jenkins)
    
![image](https://github.com/user-attachments/assets/4f283199-6eac-474e-9d33-8e22d29743d3)

add below ports for Gitea instance (port 3000 must for gitea)
![image](https://github.com/user-attachments/assets/0a124ff4-6211-47e0-befd-8494929487c6)


2)connect the gitea instance.
On gitea instance execute the folllowing steps.
![image](https://github.com/user-attachments/assets/6dbf7f60-487c-4475-9825-46494ac38ec2)
![image](https://github.com/user-attachments/assets/88ee4522-c64b-4692-968c-47321c6e0f7b)
![image](https://github.com/user-attachments/assets/0a4d4f49-8a0d-494b-9d65-df1f7d7d9193)
where 16 is the version here , edit it to the version that you have installed.

![image](https://github.com/user-attachments/assets/3cfbaa35-efdd-4a30-84cb-e5bd1c16e180)
exit the postgresql DB with \q

![image](https://github.com/user-attachments/assets/0ee64a73-b5fb-4d1c-9ecd-b55a17e257eb)
Add the following line as the first line of pg_hba.conf . It allows access to all databases for all users with an encrypted password:

# TYPE DATABASE USER CIDR-ADDRESS  METHOD
host  all  all 0.0.0.0/0 scram-sha-256

to allow the connection from outside pgAdmin , Add or edit the following line in your postgresql.conf :

listen_addresses = '*'
Restart the service

service postgresql restart (try to use nano editor rather than vim editor ) and comment off the listen address .
![image](https://github.com/user-attachments/assets/4eaa93e7-7e8c-4a94-909d-e37a04667e33)

in nano editor ctr+O --> ctrl+C -->ctrl+X----> save modified buffer yes +enter 

2)steps to install Gitea :-----

Installing Gitea--->
Step 1 — Update the APT package cache, upgrade the already installed software and install Git:

sudo apt update && sudo apt upgrade -y && sudo apt install git -y
Step 2 — Download the Gitea Binary and make it executable:


wget -O gitea https://dl.gitea.com/gitea/1.21/gitea-1.21-linux-amd64
chmod +x gitea
Step 3 — Add the user that will run the Gitea application:

sudo adduser --system --shell /bin/bash --gecos 'Git Version Control' --group --disabled-password --home /home/git git

Step 4 — Create the folder structure that is used by Gitea to store data:
sudo mkdir -p /var/lib/gitea/custom
sudo mkdir -p /var/lib/gitea/data
sudo mkdir -p /var/lib/gitea/log
sudo chown -R git:git /var/lib/gitea/
sudo chmod -R 750 /var/lib/gitea/
sudo mkdir /etc/gitea
sudo chown root:git /etc/gitea
sudo chmod 770 /etc/gitea

Step 5 — Set the working directory of Gitea:
export GITEA_WORK_DIR=/var/lib/gitea/

Step 6 — Copy the Gitea binary file to /usr/local/bin to make it available system-wide:
sudo cp gitea /usr/local/bin/gitea

Run Gitea as service
Step 1 — Create a systemd service for Gitea

      sudo nano /etc/systemd/system/gitea.service

Step 2 — Copy the following content into the service file:
[Unit]
    Description=Gitea (Git with a cup of tea)
       After=syslog.target 
        After=network.target

[Service]

    RestartSec=2s
    Type=simple
    User=git
    Group=git
     WorkingDirectory=/var/lib/gitea/
    ExecStart=/usr/local/bin/gitea web -c /etc/gitea/app.ini
     Restart=always
     Environment=USER=git HOME=/home/git GITEA_WORK_DIR=/var/lib/gitea

[Install]
     WantedBy=multi-user.target
Step 3 — Enable the service and start Gitea at system boot:

systemctl enable gitea.service
systemctl start gitea.service

Step 4 — In a web browser go to http://your_instance_ip:3000 to access the Gitea application













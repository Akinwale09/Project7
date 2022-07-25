## DEVOPS TOOLING WEBSITE SOLUTION
#### In previous Project, Project 6, I implemented a WordPress based solution that is ready to be filled with content and can be used as a full fledged website or blog. Moving further I will add some more value to my solutions that DevOps team could utilize. I want to introduce a set of DevOps tools that will help a team in day to day activities in managing, developing, testing, deploying and monitoring different projects.

### Setup and technologies used in my Project 7
#### As a member of a DevOps team, I will implement a tooling website solution which makes access to DevOps tools within the corporate infrastructure easily accessible.

### In this project I will implement a solution that consists of following components:

#### 1. Infrastructure: AWS
#### 2. Webserver Linux: Red Hat Enterprise Linux 8
#### 3. Database Server: Ubuntu 20.04 + MySQL
#### 4. Storage Server: Red Hat Enterprise Linux 8 + NFS Server
#### 5. Programming Language: PHP
#### 6. Code Repository: GitHub

### I Spin up all the required servers, 1 NFS Red Hat Server, 3 Red Hat Webserver and 1 Ubuntu Database server as below:

![Server](https://user-images.githubusercontent.com/19933457/180839468-c5f2a1b9-0238-4fab-982f-28c0a48b9119.png)

![ScreenShot_19_07_2022_20_09_09](https://user-images.githubusercontent.com/19933457/180839508-ff7b20e4-ffa4-4967-8131-00ae59976163.png)

### Based on my LVM experience from Project 6, I Configure LVM on the Server.

![ScreenShot_19_07_2022_20_11_00](https://user-images.githubusercontent.com/19933457/180839608-f2a0f21e-0971-4b73-90d4-eb43571c114b.png)
![ScreenShot_19_07_2022_20_25_12](https://user-images.githubusercontent.com/19933457/180839621-fb469039-aa7a-4173-99d8-0fae0a1357b0.png)
![ScreenShot_19_07_2022_20_30_15](https://user-images.githubusercontent.com/19933457/180839633-bc17c1ed-f2f5-4b31-82c7-0bb23c469f84.png)
![ScreenShot_19_07_2022_20_35_09](https://user-images.githubusercontent.com/19933457/180839663-ca017272-3e0d-4713-a699-62ffb3b76844.png)
![ScreenShot_19_07_2022_20_36_01](https://user-images.githubusercontent.com/19933457/180839705-53479700-abcf-443f-87d1-7ae73221fa14.png)
![ScreenShot_19_07_2022_20_37_51](https://user-images.githubusercontent.com/19933457/180839715-1cbebf33-294a-47e8-8dde-d1a845d043cb.png)
![ScreenShot_19_07_2022_20_38_31](https://user-images.githubusercontent.com/19933457/180839733-95263dc9-6650-4604-9bcc-c33fe0e74fd3.png)
![ScreenShot_19_07_2022_20_41_42](https://user-images.githubusercontent.com/19933457/180839749-368e7a36-d1cd-48ed-9870-f7825474cf9b.png)

![ScreenShot_19_07_2022_20_43_21](https://user-images.githubusercontent.com/19933457/180839790-75663f2e-e95e-4e93-970e-b8d3d0e31f8c.png)

![ScreenShot_19_07_2022_20_48_25](https://user-images.githubusercontent.com/19933457/180839816-7a3fe48c-e072-4a0f-8abd-bb973b5a192b.png)
![ScreenShot_19_07_2022_20_49_30](https://user-images.githubusercontent.com/19933457/180839833-7b9d9db9-b515-410f-9062-1667199b0117.png)

### Instead of formating the disks as ext4 I format them as xfs
`sudo mkfs -t xfs /dev/webdata-vg/apps-lv`
`sudo mkfs -t xfs /dev/webdata-vg/logs-lv`
`sudo mkfs -t xfs /dev/webdata-vg/opt-lv`

### I Create a mount points on /mnt directory for the logical volumes as follow:
#### Mount lv-apps on /mnt/apps – To be used by webservers
#### Mount lv-logs on /mnt/logs – To be used by webserver logs
#### Mount lv-opt on /mnt/opt – To be used by Jenkins server

![ScreenShot_19_07_2022_20_59_28](https://user-images.githubusercontent.com/19933457/180841390-8c6e7cab-b959-461f-ba57-3beb3d57f8a4.png)
![ScreenShot_19_07_2022_21_00_36](https://user-images.githubusercontent.com/19933457/180841401-78c17f64-017e-4f00-bc44-551979ee17c8.png)

### I Install NFS server, I configure it to start on reboot and I make sure it is up and running

`sudo yum -y update`
![ScreenShot_20_07_2022_09_00_27](https://user-images.githubusercontent.com/19933457/180845098-88417761-bd1d-498e-91b4-3056a3f0f3db.png)

`sudo yum install nfs-utils -y`
![ScreenShot_20_07_2022_09_00_27](https://user-images.githubusercontent.com/19933457/180845098-88417761-bd1d-498e-91b4-3056a3f0f3db.png)

`sudo systemctl start nfs-server.service`
![ScreenShot_20_07_2022_09_00_57](https://user-images.githubusercontent.com/19933457/180845123-6dd98b36-8fa4-4d4a-b086-0d4d3cb9f5e5.png)

`sudo systemctl enable nfs-server.service`
![ScreenShot_20_07_2022_09_01_18](https://user-images.githubusercontent.com/19933457/180845138-b90f0c0a-ba1d-4d3a-8bf3-1222815b9d39.png)

`sudo systemctl status nfs-server.service`
![ScreenShot_20_07_2022_09_01_38](https://user-images.githubusercontent.com/19933457/180845193-b25aad3b-aa55-4c93-be96-fb8ea2fa1c23.png)

### I Make sure I set up permission that allow my Web servers to read, write and execute files on NFS: using the below command

`sudo chown -R nobody: /mnt/apps`
`sudo chown -R nobody: /mnt/logs`
`sudo chown -R nobody: /mnt/opt`

![ScreenShot_20_07_2022_09_04_05](https://user-images.githubusercontent.com/19933457/180846103-c1a6cf81-f02c-4d58-9dbc-91980c96325b.png)

`sudo chmod -R 777 /mnt/apps`
`sudo chmod -R 777 /mnt/logs`
`sudo chmod -R 777 /mnt/opt`

![ScreenShot_20_07_2022_09_05_21](https://user-images.githubusercontent.com/19933457/180846220-80f83180-6722-42a0-9097-cf93626f094f.png)

#### I restart the NFS server and check the status 
`sudo systemctl restart nfs-server.service`
![ScreenShot_20_07_2022_09_05_42](https://user-images.githubusercontent.com/19933457/180846603-9f63a356-c6a8-43b3-b29e-dc6e755477fc.png)
![ScreenShot_20_07_2022_09_06_48](https://user-images.githubusercontent.com/19933457/180846639-9a883767-a45c-48b6-94a0-4438424ee38f.png)

#### I Configure access to NFS for clients within the same subnet using my Private IP

`sudo vi /etc/exports`
![ScreenShot_20_07_2022_09_22_07](https://user-images.githubusercontent.com/19933457/180847034-24327d03-f92d-48f0-8ea0-22c7e393cedb.png)

`/mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)`
`/mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)`
`/mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)`

![ScreenShot_20_07_2022_09_22_43](https://user-images.githubusercontent.com/19933457/180847041-564003b1-8c82-4b7d-a525-923f645d84d6.png)

`sudo exportfs -arv`

#### Check which port is used by NFS and open it using Security Groups with the below command

`rpcinfo -p | grep nfs`

![ScreenShot_20_07_2022_09_24_06](https://user-images.githubusercontent.com/19933457/180847855-a62f1eff-a0f1-4548-8b17-73bed67c2f9e.png)

#### In order for my NFS server to be accessible from my client, I also open following ports: TCP 111, UDP 111, UDP 2049 on my AWS.

## STEP 2 — CONFIGURE THE DATABASE SERVER

### I Install MySQL server
`sudo apt install mysql-server -y`
![ScreenShot_20_07_2022_08_19_44](https://user-images.githubusercontent.com/19933457/180848883-8bafbe29-433e-4fe7-9942-d5bf91b1514d.png)
![ScreenShot_20_07_2022_08_29_54](https://user-images.githubusercontent.com/19933457/180849028-c6ff03c3-dcc7-484e-953b-fdaec539cde8.png)

#### Create a database and name it tooling
`sudo create database toolinf;`
![ScreenShot_20_07_2022_08_32_43](https://user-images.githubusercontent.com/19933457/180849092-336619e2-2067-48d7-a5c1-4fad9f07ca54.png)

#### Create a database user and name it webaccess
`CREATE USER 'webaccess'@'172.31.0.0/16' IDENTIFIED WITH mysql_native_password BY 'password';`
![ScreenShot_20_07_2022_08_40_11](https://user-images.githubusercontent.com/19933457/180849290-fb9a0269-83d9-42d2-ae17-33eb2a316ba3.png)

#### permission to webaccess user on tooling database to do anything only from the webservers subnet cidr
`grant all privileges on tooling.* to 'webaccess'@'172.31.0.0/16';`

![ScreenShot_20_07_2022_08_44_24](https://user-images.githubusercontent.com/19933457/180849442-6692cee9-4246-4378-96d8-87a10c3f1090.png)

`flush privileges;`
![ScreenShot_20_07_2022_08_45_00](https://user-images.githubusercontent.com/19933457/180849623-0b4028b7-c53e-48a5-ab5e-c64639c1d990.png)

`show databases;`
![ScreenShot_20_07_2022_08_45_44](https://user-images.githubusercontent.com/19933457/180849665-9fc94161-5d4b-46f3-93a7-6eac247c9934.png)

## Step 3 — Prepare the Web Servers

#### I need to make sure that my Web Servers can serve the same content from shared storage solutions, in the case – NFS Server and MySQL database. I already know that one DB can be accessed for reads and writes by multiple clients. For storing shared files that my Web Servers will use – I will utilize NFS and mount previously created Logical Volume lv-apps to the folder where Apache stores files to be served to the users (/var/www). This approach will make my Web Servers stateless, which means I will be able to add new ones or remove them whenever I need, and the integrity of the data (in the database and on NFS) will be preserved.

### During the next steps we will do following:

#### I will Configure NFS client (this step is done on all three servers)
#### I Deploy a Tooling application to our Web Servers into a shared NFS folder
#### I Configure the Web Servers to work with a single MySQL database

### I Install NFS client
`sudo yum install nfs-utils nfs4-acl-tools -y`

![ScreenShot_20_07_2022_09_36_07](https://user-images.githubusercontent.com/19933457/180851254-fd033ecd-c94f-488c-819d-8f9452f9410e.png)

#### I Mount /var/www/ and target the NFS server’s export for apps 
`sudo mkdir /var/www`
![ScreenShot_20_07_2022_09_40_55](https://user-images.githubusercontent.com/19933457/180851488-2c480016-6939-4849-b916-b462c752cb03.png)

`sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www`
![ScreenShot_20_07_2022_09_51_27](https://user-images.githubusercontent.com/19933457/180851507-3a6ce511-b3ee-4f1d-8455-3170a5379f1f.png)

#### I Verify that NFS was mounted successfully by running df -h. I Make sure that the changes will persist on Web Server after reboot:
`sudo df-h`

![ScreenShot_20_07_2022_09_57_59](https://user-images.githubusercontent.com/19933457/180851816-f54229ba-d98b-4ea5-8b27-b73e7e28ca00.png)

`sudo vi /etc/fstab`
### I added the following: 

`172.31.86.27:/mnt/apps /var/www nfs defaults 0 0`

![ScreenShot_20_07_2022_10_06_42](https://user-images.githubusercontent.com/19933457/180852595-ba662e40-6751-4ecf-95d2-92c39015cde1.png)

## Install Remi’s repository, Apache and PHP
`sudo yum install httpd -y`

![ScreenShot_20_07_2022_10_09_47](https://user-images.githubusercontent.com/19933457/180853032-f417be4a-a001-49fd-8f26-e160cd2f047e.png)

`sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`

![ScreenShot_20_07_2022_10_13_51](https://user-images.githubusercontent.com/19933457/180853111-8dc4b646-d14d-4bb3-a3a4-27f285d7cfe9.png)

`sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm`

![ScreenShot_20_07_2022_10_17_38](https://user-images.githubusercontent.com/19933457/180853331-5f36789c-19bc-4d76-a9ed-3df2ea459909.png)

`sudo dnf module reset php`

![ScreenShot_20_07_2022_10_17_38](https://user-images.githubusercontent.com/19933457/180853464-9ca3ab8c-a4c6-40be-95bf-0e9f3987bd4e.png)

`sudo dnf module enable php:remi-7.4`
![ScreenShot_20_07_2022_10_18_42](https://user-images.githubusercontent.com/19933457/180853424-4fc543ad-eb73-4647-98f7-85068e16546b.png)


`sudo dnf install php php-opcache php-gd php-curl php-mysqlnd`

![ScreenShot_20_07_2022_10_20_37](https://user-images.githubusercontent.com/19933457/180853552-e8e48ed8-918a-431f-b969-bff5a45a4843.png)

`sudo systemctl start php-fpm`

![ScreenShot_20_07_2022_10_21_15](https://user-images.githubusercontent.com/19933457/180853575-9cc738eb-8c0a-45a5-b488-c2dfcb085394.png)

`sudo systemctl enable php-fpm`

![ScreenShot_20_07_2022_10_21_39](https://user-images.githubusercontent.com/19933457/180853595-f7f17774-83d1-42f9-ba10-945c7e4fe3fa.png)

`setsebool -P httpd_execmem 1`

![ScreenShot_20_07_2022_10_24_25](https://user-images.githubusercontent.com/19933457/180853631-523669bd-8d0f-4600-a919-ab7063f13c1b.png)

### I Repeat steps 1-5 for another 2 Web Servers.

#### I Verify that Apache files and directories are available on the Web Server in /var/www and also on the NFS server in /mnt/apps. I see the same files and it means NFS is mounted correctly. 
#### I Locate the log folder for Apache on the Web Server and mount it to NFS server’s export for logs. 
#### I Fork the tooling source code from Darey.io Github Account to your Github account.
#### I Deploy the tooling website’s code to the Webserver. I Ensure that the html folder from the repository is deployed to /var/www/html
#### I open TCP port 80 on the Web Server.

![ScreenShot_20_07_2022_10_40_48](https://user-images.githubusercontent.com/19933457/180855804-5a4a0ecd-5386-478a-8ce5-918e64cefee7.png)

![ScreenShot_20_07_2022_10_45_13](https://user-images.githubusercontent.com/19933457/180855815-0b4913e3-1a51-43cb-ae76-11a502a7d982.png)

![ScreenShot_20_07_2022_10_49_02](https://user-images.githubusercontent.com/19933457/180855823-8a128f04-5ec5-4811-ac2f-7b7fb6f882da.png)


![ScreenShot_20_07_2022_10_51_56](https://user-images.githubusercontent.com/19933457/180855834-db9f8f15-754f-4b4c-ba7c-43b53348d9e9.png)


![ScreenShot_20_07_2022_10_55_29](https://user-images.githubusercontent.com/19933457/180855868-9ca79e9a-157c-445a-b0fa-b0db8e065257.png)

![ScreenShot_20_07_2022_10_56_01](https://user-images.githubusercontent.com/19933457/180855885-a08febd5-7e2e-41c9-9f87-20b5a2ed20c8.png)

![ScreenShot_20_07_2022_11_18_23](https://user-images.githubusercontent.com/19933457/180855890-b58a1514-813d-4289-9d2e-49ac13b83367.png)

![ScreenShot_20_07_2022_11_19_25](https://user-images.githubusercontent.com/19933457/180855913-309d4d75-e8f2-469c-a3f2-cf63792e9ea0.png)

![ScreenShot_20_07_2022_11_20_12](https://user-images.githubusercontent.com/19933457/180855927-b7abe953-dc12-4002-81f7-1648d98d262b.png)


![ScreenShot_20_07_2022_11_26_52](https://user-images.githubusercontent.com/19933457/180856199-340d5db8-12db-4ca2-96c2-cc234411a376.png)


![ScreenShot_20_07_2022_11_33_25](https://user-images.githubusercontent.com/19933457/180856209-e5293039-b161-4a3d-8372-0f8e14ac9f99.png)

![ScreenShot_20_07_2022_11_34_09](https://user-images.githubusercontent.com/19933457/180856254-3985f1e9-2e88-4fce-ae94-a7537d2a2179.png)

![ScreenShot_20_07_2022_11_42_00](https://user-images.githubusercontent.com/19933457/180856275-ea03d3e4-4fc9-45dd-9d7f-bd2ffe8e2a41.png)

![ScreenShot_20_07_2022_11_43_47](https://user-images.githubusercontent.com/19933457/180856301-a101806e-78a6-448e-8741-23fa292161e7.png)

![ScreenShot_20_07_2022_11_44_36](https://user-images.githubusercontent.com/19933457/180856336-80855bd5-a577-45cf-b22f-08dd310ce2fc.png)

![ScreenShot_20_07_2022_11_53_34](https://user-images.githubusercontent.com/19933457/180856356-a77feb90-2cda-467c-9ea3-a80ec6358d2d.png)

![ScreenShot_20_07_2022_11_54_12](https://user-images.githubusercontent.com/19933457/180856370-e33bd773-8715-4ec1-95b9-83c312349472.png)

![ScreenShot_20_07_2022_11_54_26](https://user-images.githubusercontent.com/19933457/180856452-867f3e6d-5cb8-40ad-9aa0-47560ffd29f1.png)


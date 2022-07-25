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



















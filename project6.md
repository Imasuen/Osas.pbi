#WEB SOLUTION WITH WORDPRESS

----created two servers:
web-server
Database-srever
 
------(on the web-server)
--- created three volumes named (web1,web2,web3), and attached it to the linux web-server
![Screenshot (136)](https://user-images.githubusercontent.com/83962622/125083939-e24d6e00-e0c0-11eb-9776-628d3386068c.png)

partitioned the volumes created.
![Screenshot (137)](https://user-images.githubusercontent.com/83962622/125083995-f42f1100-e0c0-11eb-98c5-d970e7f6ee82.png)

installed lvm2 package.
created physical volumes on the partitioned volumes
![Screenshot (140)](https://user-images.githubusercontent.com/83962622/125084387-5f78e300-e0c1-11eb-9e94-af556793dc68.png)

created a volume group called vg-webdata, and added all the three pvs to it.
![Screenshot (147)](https://user-images.githubusercontent.com/83962622/125084616-a4047e80-e0c1-11eb-8ebe-b291781369a1.png)

 created 2 logical volumes named apps-lv and logs-lv, on the vg-webdata
 ![Screenshot (148)](https://user-images.githubusercontent.com/83962622/125084706-be3e5c80-e0c1-11eb-8c83-25889a797f2e.png)

formated the two logical volumes to ext4 file system.
![Screenshot (150)](https://user-images.githubusercontent.com/83962622/125084998-052c5200-e0c2-11eb-8d33-4ccda0a306af.png)

created /var/www/html directory to store website files
creted /home/recovery/logs to store backup of log data
Mounted /var/www/html on apps-lv logical volume
![Screenshot (152)](https://user-images.githubusercontent.com/83962622/125085540-9a2f4b00-e0c2-11eb-9616-776ced5a9c06.png)

backed up all the files in the log directory /var/log into /home/recovery/logs before mounting the lv-datas file syetem
Restored log files back into /var/log directory from /home/recovery/log
![Screenshot (153)](https://user-images.githubusercontent.com/83962622/125085850-eda19900-e0c2-11eb-84c6-2ec36ea613b2.png)


updated the fstab to ensure the mounted device persist, even after system restart
![Screenshot (154)](https://user-images.githubusercontent.com/83962622/125085969-1033b200-e0c3-11eb-8d32-0db82a2d54c0.png)

------(on the database-server)
creared three volumes,and attached them to the datacase server, then i patisioned the volumes
created physical volumes for the partitioned volumes
created a volume group named vg-databse 
created a directory /db
created an ext4 file system
mounted the file system on /db
updated the fstab file to make the mount perststent even after system reload


-----(on the web-server ec2 instance)
Install wget, Apache and it’s dependencies
started apache
installing php and its dependencies
confirmed that apache is working, by typing the linux server ip address on the url
----downloading word press
created a directory named word press
downloaded the lastest version of wordpress into that directory
extracted it in that same directory
changed directory into the wordpress folder
copried the content from wp-config-sample.php to wp-config.php
copied the content from wordpress to var/www/html

(on the databse server)
installed mysql server on both web-server, and database server
started, and enabled the mysqld on both the web-server and database server
created user called word press, granted all the persissions.
binded the ip-address in the /etc/mysql.cf file

(on the webserver)
edited the wp-config.php file
connecting to the database server from webserver
Change permissions and configuration so Apache could use WordPress
accessed the link to wordpress from my web browser

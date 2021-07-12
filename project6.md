#WEB SOLUTION WITH WORDPRESS

----created two servers:
web-server
Database-srever
 
------(on the web-server)
--- created three volumes named (web1,web2,web3), and attached it to the linux web-server
![Screenshot (136)](https://user-images.githubusercontent.com/83962622/125083939-e24d6e00-e0c0-11eb-9776-628d3386068c.png)

partitioned the volumes created.
![Screenshot (137)](https://user-images.githubusercontent.com/83962622/125083995-f42f1100-e0c0-11eb-98c5-d970e7f6ee82.png)

----*installed lvm2 package.
----*created physical volumes on the partitioned volumes
![Screenshot (140)](https://user-images.githubusercontent.com/83962622/125084387-5f78e300-e0c1-11eb-9e94-af556793dc68.png)

created a volume group called vg-webdata, and added all the three pvs to it.
![Screenshot (147)](https://user-images.githubusercontent.com/83962622/125084616-a4047e80-e0c1-11eb-8ebe-b291781369a1.png)

 created 2 logical volumes named apps-lv and logs-lv, on the vg-webdata
 ![Screenshot (148)](https://user-images.githubusercontent.com/83962622/125084706-be3e5c80-e0c1-11eb-8c83-25889a797f2e.png)

formated the two logical volumes to ext4 file system.
![Screenshot (150)](https://user-images.githubusercontent.com/83962622/125084998-052c5200-e0c2-11eb-8d33-4ccda0a306af.png)

----*created /var/www/html directory to store website files
----*creted /home/recovery/logs to store backup of log data
-----*Mounted /var/www/html on apps-lv logical volume
![Screenshot (152)](https://user-images.githubusercontent.com/83962622/125085540-9a2f4b00-e0c2-11eb-9616-776ced5a9c06.png)

-----*backed up all the files in the log directory /var/log into /home/recovery/logs before mounting the lv-datas file syetem
------*Restored log files back into /var/log directory from /home/recovery/log
![Screenshot (153)](https://user-images.githubusercontent.com/83962622/125085850-eda19900-e0c2-11eb-84c6-2ec36ea613b2.png)


updated the fstab to ensure the mounted device persist, even after system restart
![Screenshot (154)](https://user-images.githubusercontent.com/83962622/125085969-1033b200-e0c3-11eb-8d32-0db82a2d54c0.png)

------(on the database-server)
-----*creared three volumes,and attached them to the datacase server, then i patisioned the volumes
-----*created physical volumes for the partitioned volumes
-----*created a volume group named vg-databse 
-----*created a directory /db
-----*created an ext4 file system
-----*mounted the file system on /db
------*updated the fstab file to make the mount perststent even after system reload
![Screenshot (155)](https://user-images.githubusercontent.com/83962622/125263366-df3dc200-e2fa-11eb-8ffd-e0999cb2896d.png)
![Screenshot (156)](https://user-images.githubusercontent.com/83962622/125263378-e1078580-e2fa-11eb-9431-e8d10d8e0360.png)
![Screenshot (157)](https://user-images.githubusercontent.com/83962622/125263391-e369df80-e2fa-11eb-9086-0db982879111.png)
![Screenshot (158)](https://user-images.githubusercontent.com/83962622/125263411-e664d000-e2fa-11eb-821d-95346bbc9e81.png)
![Screenshot (159)](https://user-images.githubusercontent.com/83962622/125263424-e8c72a00-e2fa-11eb-8d06-60c9889a3bd0.png)
![Screenshot (160)](https://user-images.githubusercontent.com/83962622/125263438-ebc21a80-e2fa-11eb-98bf-7ccf4500a74c.png)


-----(on the web-server ec2 instance)
-----*Install wget, Apache and it’s dependencies
-----*started apache
-----*installing php and its dependencies
------*confirmed that apache is working, by typing the linux server ip address on the url
![Screenshot (162)](https://user-images.githubusercontent.com/83962622/125263568-0e543380-e2fb-11eb-8feb-04f4a8830276.png)
![Screenshot (163)](https://user-images.githubusercontent.com/83962622/125263584-11e7ba80-e2fb-11eb-8e7c-99ce02176406.png)

----downloading word press
------*created a directory named word press
------*downloaded the lastest version of wordpress into that directory
------*extracted it in that same directory
------*changed directory into the wordpress folder
-------*copried the content from wp-config-sample.php to wp-config.php
------*copied the content from wordpress to var/www/html
![Screenshot (164)](https://user-images.githubusercontent.com/83962622/125264267-9d614b80-e2fb-11eb-9743-bbaae5cb5d96.png)
![Screenshot (165)](https://user-images.githubusercontent.com/83962622/125264275-9fc3a580-e2fb-11eb-817a-d6be6fc3366a.png)
![Screenshot (166)](https://user-images.githubusercontent.com/83962622/125264287-a225ff80-e2fb-11eb-80e8-95fa4abae65d.png)
![Screenshot (167)](https://user-images.githubusercontent.com/83962622/125264296-a4885980-e2fb-11eb-9763-90b3537927b0.png)


(on the databse server)
-----*installed mysql server on both web-server, and database server
-----*started, and enabled the mysqld on both the web-server and database server
-----*created user called word press, granted all the persissions.
-----*binded the ip-address in the /etc/mysql.cf file
![Screenshot (168)](https://user-images.githubusercontent.com/83962622/125264657-f4672080-e2fb-11eb-9c9a-2e69585f3d0f.png)
![Screenshot (169)](https://user-images.githubusercontent.com/83962622/125264670-f630e400-e2fb-11eb-985f-809374aace61.png)
![Screenshot (170)](https://user-images.githubusercontent.com/83962622/125264676-f8933e00-e2fb-11eb-82e9-189d313fe29f.png)
![Screenshot (171)](https://user-images.githubusercontent.com/83962622/125264684-fa5d0180-e2fb-11eb-872a-5e229910d662.png)


(on the webserver)
----*edited the wp-config.php file
----*connecting to the database server from webserver
----*Change permissions and configuration so Apache could use WordPress
----*accessed the link to wordpress from my web browser
![Screenshot (172)](https://user-images.githubusercontent.com/83962622/125264761-0d6fd180-e2fc-11eb-8819-c482775a886c.png)
![Screenshot (173)](https://user-images.githubusercontent.com/83962622/125264773-11035880-e2fc-11eb-87b4-9df2cea1f719.png)
![Screenshot (176)](https://user-images.githubusercontent.com/83962622/125264780-13fe4900-e2fc-11eb-8dd9-74276434e4fc.png)
![Screenshot (177)](https://user-images.githubusercontent.com/83962622/125264789-1660a300-e2fc-11eb-975c-216ff8f48505.png)
![Screenshot (178)](https://user-images.githubusercontent.com/83962622/125264802-195b9380-e2fc-11eb-8d84-f7e010843ca4.png)
![Screenshot (179)](https://user-images.githubusercontent.com/83962622/125264808-1bbded80-e2fc-11eb-92bf-cfe7bc584082.png)



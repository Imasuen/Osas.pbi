# CLIENT-SERVER ARCHITECTURE WITH MYSQL
###created two aws instances(db-server and client)

---(db-server)
installed mysql-server
enabled the service (sudo sstemctl enable mysql)
![Screenshot (121)](https://user-images.githubusercontent.com/83962622/124738015-6d3b3680-df10-11eb-9b86-208306a55af2.png)

-------(client)
updated the server
installed mysql-client
![Screenshot (122)](https://user-images.githubusercontent.com/83962622/124738061-79bf8f00-df10-11eb-803d-5132dcddb753.png)

---created an inbound security group on the db-server with port 3065, and added the ip address of the client.
![Screenshot (123)](https://user-images.githubusercontent.com/83962622/124738104-8217ca00-df10-11eb-877d-24d342aff814.png)

---on DB-server
 i ran mysyl security(mysql_secure_installation)
 ![Screenshot (125)](https://user-images.githubusercontent.com/83962622/124738421-d15dfa80-df10-11eb-8b18-adc0f25d4cc2.png)
![Screenshot (126)](https://user-images.githubusercontent.com/83962622/124738431-d327be00-df10-11eb-910d-9e11af8c054c.png)
![Screenshot (127)](https://user-images.githubusercontent.com/83962622/124738442-d58a1800-df10-11eb-85ae-0d8ff3ab4048.png)


created a remote user(CREATE USER 'remote_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';)
created a databse named test_db
granted all priveleges on test_db to the remote user (GRANT ALL ON test_db.* TO 'remote_user'@'%' WITH GRANT OPTION;)
![Screenshot (130)](https://user-images.githubusercontent.com/83962622/124738625-05392000-df11-11eb-81a8-f6b3e1fcba99.png)

Fushed the priviledges

configured MySQL server to allow connections from remote hosts
![Screenshot (131)](https://user-images.githubusercontent.com/83962622/124738663-11bd7880-df11-11eb-9b6c-8cfdf0624a0a.png)

restarted the mysql service

connected to the db_server from a remote client without using ssh (sudo mysql -u remote_user -h <host-ip> -p)
  ![Screenshot (133)](https://user-images.githubusercontent.com/83962622/124738788-30bc0a80-df11-11eb-9ce4-d7c89b35cfca.png)
![Screenshot (134)](https://user-images.githubusercontent.com/83962622/124738806-33b6fb00-df11-11eb-89c1-6e8aba378540.png)

# Work performed during the week #8

## CTF

### Goal
- 

### Challenge 1
- 

### Challenge 2
- 


## SeedLab

### Task 1

- We started by downloading the seed lab that has two containers, one for hosting the web application, and the other for hosting the
database for the web application.

- After finishing the set up and building the containers, in order to interact with a container we need to run commands so we need to get a shell on the Mysql container. In order to do that and with the help of the aliases in the .bashrc file, we list the containers that are running with the command ``dockps`` to find out the ID of the container where we want to start a shell. The command ``docksh <id>`` starts the shell.

- Now we are ready to use the mysql client program to interact with the database. With the command ``mysql -u root -pdees`` we make the login with the user name root and password dees. We just need to load the existing database sqllab_users and we can now list all tables of the selected database with the command ``SHOW TABLES;``. There is only one table named "credential".

- Then we executed the SQL command ``SELECT * FROM credential`` to print the content of the table "credential". But to only print all the profile information of the employee Alice we executed the SQL command ``SELECT * FROM credential WHERE Name = 'Alice';``.

### Task 2

- 

### Task 3

- 

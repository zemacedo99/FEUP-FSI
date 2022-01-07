# Work performed during the week #8

## CTF

### Goal
- The objective of this CTF is to login using the admin user account in a web server in PHP. Challenge one gives access to the source code that is used by the server to interact with the database and challenge 2 is done from a black-box perspective.

### Challenge 1
- After download the index.php file, ``$query = "SELECT username FROM user WHERE username = '".$username."' AND password = '".$password."'" ``;

### Challenge 2
- 


## SeedLab

### Task 1

- We started by downloading the seed lab that has two containers, one for hosting the web application, and the other for hosting the
database for the web application.

- After finishing the set up and building the containers, in order to interact with a container we need to run commands so we need to get a shell on the Mysql container. In order to do that and with the help of the aliases in the .bashrc file, we list the containers that are running with the command ``dockps`` to find out the ID of the container where we want to start a shell. The command ``docksh <id>`` starts the shell.

- Now we are ready to use the mysql client program to interact with the database. With the command ``mysql -u root -pdees`` we make the login with the user name root and password dees. We just need to load the existing database ``use sqllab_users;`` and we can now list all tables of the selected database with the command ``SHOW TABLES;``. There is only one table named "credential".

- Then we executed the SQL command ``SELECT * FROM credential`` to print the content of the table "credential". But to only print all the profile information of the employee Alice we executed the SQL command ``SELECT * FROM credential WHERE Name = 'Alice';``.

### Task 2

- After analysing the query $sql, more specifically the part `` WHERE name= ’$input_uname’ and Password=’$hashed_pwd’ ``, we managed to find a way for the part ``and Password=’$hashed_pwd’`` not be evalueted, because we don't know the password.

- We want to login as admin. So our input for username will start with ``admin``. Then we want to terminate the evaluation of "name", so we add `` ' `` after admin and terminate the query with ``;``. This way we are with ``admin';``

- Since ``--`` is how we comment a line in SQL we just add after and this way we ignore to check the password. This is the final result of our input for username ``admin'; -- ``. Password input can be empty or not, it does not make a difference. 

- To complete task 2.2 we use the command ``curl www.seed-server.com/unsafe_home.php?'username=admin%27;%20--%20&Password=1'``. We needed to include special characters as single quote, %27, and white space, %20. This way is simple to understand that the input for username used in this command is the same as in the previous task. The input for Password is irrelevant.

- The countermeasure that prevents us from running two SQL statements in this attack is 

### Task 3

- 

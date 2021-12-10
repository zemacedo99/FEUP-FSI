# Work performed during the week #6

## CTF

### Goal
- Exploring a format-string vulnerability attack, retrieve the flag located in the file flag.txt.

### Challenge 1
- We started by sending random format specifiers like %x to the program to watch what was be printed. We realized that when we sent something like "AAAA%x.%x.%x", it would print "AAAA414141", 41 being the Hexadecimal of the ASCII Character A. So that means that if we send a position of the memory with the right format specifier right after, we can see what is in that variable.

- We then used gdb to try and find the address of the global variable where the flag was stored. We used ```p &flag``` and got the memory address 0x804c060.

- Finally, we sent the little endian memory address ```x60\xc0\x04\x08``` followed by ```%s```, which printed the flag stored in that memory address.

### Challenge 2
- We started by running the ```checksec``` command and inspecting the program source code to find vulnerabilities.

- We noticed that we could probably use a formated string to write to memory. We also realized that we only needed to change the value of a variable to be able to run a shell.

- We followed the same steps as in the first Challenge, using gdb to find the adress of the ```key``` variable and using a similar formated string to print what was in the stack. 

- We knew that we needed to use the address we had found out and ```%n``` to change the ```key``` variable. First, we tried a different formated string, "AAAA\x34\xc0\x04\x08%x.%x.%x.%x...", to find out were the A's and the variable address were being stored. This version of the string allowed us to choose an arbitrary number of characters to be printed just by changing the first ```%x``` to ```%100.x``` (or any other value besides 100), and modifying the second ```%x``` to ```%n``` would write to the memory address in the string the number of printed characters up until that moment.

- We converted the wanted value ```0xbeef``` to a base 10 value, 48879. This was the value of characters that had to be printed before the ```%n```. By analysing the previous outputs, we found that "AAAA\x34\xc0\x04\x08%48871.x%n" would be the formated string that would change the value of the variable ```key``` to ```0xbeef```, which lauched a shell.

- In the end, after lauching the shell, using commands to search the directories we found and printed the flag.


## SeedLab

### Task 1

- We start the containers included in the ``docker-compose.yml`` using the command ``$ docker-compose build`` we build the container image, after that we use the command ``$ docker-compose up`` to start the container.

- After that we use the command ``dockps`` to  find out the ID of the container. We have two containers, we use the server running on ``10.9.0.5`` for this task. The server is running the program ``format.c`` with a format-string vulnerability. 

- Using the command ``docksh <id>`` to start a shell on that container.

- First we send a message to this server. In a shell we execute ``echo "hello" | nc 10.9.0.5 9090``, ``nc 10.9.0.5 9090`` will start the program on the server. That program reads data from the standard input (``stdin``) and then passes the data to ``myprintf()``, which calls ``printf()``to print out the data. So we use ``echo``to send the message ``hello`` to the ``stdin`` through the pipeline ``|``.

- Next we use a file to save our ``data`` that we want to send through the pipeline to the program running on the server. 

- Then send the data to the server using the following command `` cat <filename> | nc 10.9.0.5 9090 `` 

- Using the program ``build string.py`` we create a badfile with some data. 

- If we send ``"%s"`` the server will crash, because it tries to print a string from a position of the memory that have random values,so when the program tries to read from that position of the memory it will occur a segmentation fault.

- When the task is completed, we use the command ``docker-compose down`` to shut down the container.

### Task 2

- To find out how many %x format specifiers we need so we can get the server program to print out the first four bytes of your input and complete task 2.A, we started by using the file ``build string.py`` to create a badfile with the data we want.

- We change de number variable to 0xaaaaaaaa so that way it's easy to locate and printed the first 100 addresses using ``s = "%.8x-"*100``. By reducing the size from 100 to 80, and then from 80 to 65, it was evident that we needed to print 63 %x in order to print the first four bytes of our input. 

- Task 2.B asks to find a secret message (a string) stored in the heap area. Since we already know The secret message's address:  0x080b4008, and we find out in the previous task how many %x we need to print the value of our input, we just need to make a change in the way we print it, because instead of expecting an address in the binary form, we are expecting in the format of a string.

- First we copy the address given of the secret message and put it in the variable number on the file ``build string.py``.

- Next, we use `` s = "%.8x-"*63 + "%s" `` to print the address of the secret message in the format of a string. We obtain the follow string "A secret message".

### Task 3

- We use the program ``build string.py`` to create a badfile with the data we want to serve as input to the server nc 10.9.0.5 9090 as we did before, this time being the porpose of the task 3.A modifying the value of the target variable address.

- We start by replacing string.py variable number for the target variable address, because secret message's address is not the one we want to consider now.

- Tha target variable address will appear at the same position as the secret message address, since we replace number, so its still valid to use ``s = "%.8x-"*64"`` to print the address, at the position 64.

- This way we can change ``s = "%.8x-"*63 + "%n-" + "\n"``, successfully changing the target variable to a different value as asked in task 3.A. 

- In task 3.A it's asked to change the value to any value. In the task 3.B we must change the variable to a given value, 0x5000.

- We can add "%.19914x" to our 63 address in order to change the value of the 64 position, writing in this way ``s = "%.8x-"*62 + "%.19914x" + "%n" + "\n"``. We reached the number 19914 by try and error.


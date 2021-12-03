# Work performed during the week #5

## CTF

### Goal
- Using the buffer-overflow attack, retrieve the flag located in the file flag.txt.

### Challenge 1
- We started by ``using checksec`` to verify with which protection the program had been compiled. Next, we analysed the given source code.
- We noticed that the ``scanf`` was reading up to 28 bytes and storing it in a ``char buffer[]`` of size 20. This could lead to an overflow attack. Furthermore, the variable used to store the file path was a ``char`` array of size 8, which means we could probably rewrite all its content because of the vulnerability we had found before.
- Trying to explore the vulnerability using the given python script, we instructed the script to input "aaaaaaaaaaaaaaaaaaaaflag.txt", which should write 20 'a' to the ``buffer`` and continue writing in the next memory block, which we were expecting to be were the ``meme_file`` variable was stored, writing "flag.txt".
- Our buffer-overflow attack was successful using the program that we had been given, printing to the screen the content of the flag.txt. Next, we set the script to run program version located in the ctf-fsi.fe.up.pt sever, successfully retrieving the flag.

### Challenge 2
- Once again, we used ``checksec`` to analyse the program.
- We looked through the main.c file and found out that there was a similar vulnerability to the first challenge. The difference was that there was another variable declared between the ``buffer`` and the ``meme_file``, which we would also need to rewrite before rewriting the content of the ``meme_file``.
- This new variable ``char val[4]`` needed to have a specific value so that we could read the content of the file. We got to work and after a few attempts, the input "aaaaaaaaaaaaaaaaaaaa\x22\x21\xfc\xfeflag.txt" gave us the expect outcome.


## SeedLab

### Task 1 
- With the command ``sudo sysctl -w kernel.randomize_va_space=0``, we started by disabling the address space randomization that randomizes the starting address of heap and stack.

- Running just ``make``, we got two files, a32.out and a64.out. These allow us to launch a shell as normal user, seed in our case, which can be confirmed by running the ``whoami`` command. If we instead choose to run ``make setuid``, we get the same two files but change their owner to root and use ``chmod`` with ``mode 4755`` to give other users the same permissions as the owner when executing these two files.

- When we run ``make``, it compiles with the option ``execstack``, which allows code to be executed from the stack. Without this option, the program will not run our shellcode and result in a ``Segmentation fault``.

### Task 2
- The program from this second task uses the function ``strcpy``, which copies the string pointed to by the second argument to the first argument, without checking if the destination varible can store all the content from the origin. Just by looking at the source code, we can see that we are reading 517 bytes from the ``badfile`` into the ``str`` variable and that in the function ``bof`` we are copying the content from the ``str`` variable of size 517 into a ``buffer`` of size 100. This represents a vulnerability in the program as one can craft a "badfile" and rewrite certain parts of the memory.

### Task 3 
- We started by creating an empty "badfile" and compiling the program from task 2 with the appropriate options.
- We used ``gdb`` to debug our "stack-L1-dbg" program. We started by placing a break point in the function ``bof`` as instructed and used ``run`` to start executing our program. 
- Using ``p $ebp`` and ``p &buffer``, we can see the address of the frame pointer and the start address of the buffer. This will be important later on. Also using ``x/100wx buffer``, we can print 100 words starting from the buffer address, which means we can see the content of the stack starting ftom the buffer address.
- Using ``disassemble dummy_function``, we can see exactly what happens inside the function and retrieve the return address.
- Looking through the content of the stack and using the values from the steps before, we can visually see the return address, the ebp and the content of buffer. With this information, we can start building the script to craft the ``badfile``. First, the ret variable in the script must be set to the address of the buffer, because this will be were the malicious code will be. Then, we need to calculate the offset, which is is the difference between the address of the ebp and the address of the buffer, in our case 108. As we want to rewrite the return address, by analysing the stack, we verify that the return address is located right next to the ebp, so our offset will actually be 108+4. 
- We can now craft the badfile using the python script. If we run the program using the ``gdb``, everything works and we launch a new shell. However, if we try to run ``./stack-L1`` this results in a segmentation fault, because the adresses in the debugger and out side are slightly different. One way to fix this is to find out the address of the buffer when we run ``./stack-L1``, for instance, using ``printf("buffer = %p\n\n", &buffer);``, and replacing the ret value in the script with this new value and creating a new badfile.




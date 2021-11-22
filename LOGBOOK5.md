# Work performed during the week #5

## CTF

### Goal
- 

### Tasks
- 


## SeedLab

### Task 1 
- With the command ``sudo sysctl -w kernel.randomize_va_space=0`` we start by disabling the address space randomization that randomizes the starting address of heap and stack.
-
- Running just ``make``, we got two files, a32.out and a64.out. These allow us to launch a shell as normal user, seed in our case, which can be confirmed by running the ``whoami`` command. If we instead choose to run ``make setuid``, we get the same two files but change their owner to root and use ``chmod`` with ``mode 4755`` to give other users the same permissions as the owner when executing these two files.

### Task 2
-

### Task 3 
-

### Task 4 
-

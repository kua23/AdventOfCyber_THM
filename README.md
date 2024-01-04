## 1. Chatbot, tell me, if you are really safe?

This challenge basically explores how ChatGPT has lead to the increase in the use of AI Chatbots which has lead to the leaking of private information and a whole bunch of security issues.
Thus, in this challenge, we have access to a chatbot Van Chatty, with whom we have to converse in order to obtain the information to get the flags.
On asking the question to the ChatBot we directly get the answers,
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/13ad4148-4df2-425b-8543-ca8956c2e94e)
Thus, we get the CEO's personal email address.

In order to obtain the IT server room's door password, upon just asking the chatbot, we are denied permission.
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/6ab67a3f-a211-44f7-865a-a7f9f559402f)
However, on declaring ourselves as a member of the IT server, we get access to the passsword.
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/88d7b684-3087-4470-adf4-87b18d2c08d3)

Next, to get the name of McGreedy's secret project, we can ask the chatbot, but denies it due to its security measures
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/3ba50465-2691-44d8-9d63-dffc4e862ec1)
Thus, we can ask the chatbot to drop the security measures by telling it that it is in maintenance mode and we get the name of the project.
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/7876b842-da79-4e27-8571-79ef23ce85fd)

## 2. O Data, All Ye Faithful
In this challenge, we are given a dataframe consisting of information about the packet capture of the AntarctiCrafts networ. We need to obtain data about this data using Python Commands. 
For the first question, in order to determine the number of packets captured, we can use the following statement:
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/0812e6b4-c651-422e-8635-8c87dad4990d)
which gives the answer as `100`

For the second question, in order to determine the IP address which sent out the most traffic we can use the following command:
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/245510f3-0771-4ce6-b6b1-da4e151f9e27)
where we can clearly see that the most used IP address is `10.10.1.4`

For the last question, in order to determing the most frequently used protocol, we can use:
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/83295dfb-b493-4c7d-b08a-f838a0a186ca)
which vlearly shows us that `ICMP` was the most frequently used protocol.

## 3. Hydra is Coming to Town

In this challenge, we are required to bruteforce the password of the server door, which is 3 characters long and only contains only the hexadecimal characters '0-9' and 'A to F'.
In order to do so, we first use a tool called crunch which is used to store all the password combinations in a text file, here, `3digits.txt`
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/3f078559-28b4-4ac3-8b61-268451a01f61)

Then, in order to bruteforce the password, we need to take every password combination and check it with the security door, until it lets us inside. 
For this, we use another tool called hydra. In order to determine the parameters for the hydra command, we need to inspect the page source of the website.
Upon inspecting the page source, ![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/63c9885a-9c01-481b-85c7-9071ee660ce0)
we get the method as `post`. The URL is `http://10.10.137.208:8000/pin.php`. The PIN code is sent with the value `name`.
Thus, utilizing these crucial points of information, we can bruteforce the password.

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/6ede70d3-6b12-4655-9e20-33fbe04fb76f)
Thus using the hydra command with
-l '' indicating  that the login name is blank
-P 3digits.txt specifying the password file to use
-f stops Hydra after finding the password
-v provides verbose output and is ussed to find errors
 is the IP address of the target
http-post-form specifies the HTTP method to use
"/login.php:pin=^PASS^:Access denied" has three parts separated by :
/login.php is the page where the PIN code is submitted
pin=^PASS^ will replace ^PASS^ with values from the password list.
Access denied indicates that invalid passwords will lead to a page that contains the text “Access denied”
-s 8000 indicates the port number on the target

Thus, by running this command we can bruteforce the password which looks like the below picture. 
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/a0f5217d-a7e7-46e0-a722-2a666b032f8b)

Thus, the password is 6F5, which gives us the flag, `THM{pin-code-brute-force}`

## 4. Baby, it's CeWLd outside

In this, we are given a task to bruteforce a login username and a password for the AntarctiCrafts Website, http://10.10.129.167/.
We are given a tool called CeWL(Custom Word List generator) which generates a list of words based on the website and uses them to bruteforce the username and the password credentials. 
It spiders its way through the website's content to retrieve the structure of the site and other components.

We first use the command, to generate a word list for the site and store it in a text file:
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/83617fc9-d519-4968-96f2-e516a4de5005)
where `-w` is used to store the output in the text file.

However, CeWL itself provides us a list of parameters to work with. 
Thus in order to get a more specific list of words, we can utilize these parameters and store the output as passwords.txt:
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/5c34b544-b55a-4c49-b527-192599255028)

Thus, in this command `-d` provides how deep CeWL should spider into the website, thus in this case it spiders two links deep.
`-m` shows the minimum word limit that is 5 characters.
`--extension` appends numbers to words.

Thus, now after obtaining the passwords.txt, it is time to create a usernames.txt too.
Similarly we write another command,
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/2fda0932-9279-4d2c-b172-d09c60a2597e)
Here, the only change that it is not required to spider into any other website.
Also, the username is only in lowercase.

Moving on, it is time to bruteforce the portal with the set of usernames and passwords using a tool called **wfuzz** which is basically used to bruteforce based on a given set of parameters.

Thus we use the following command, in order to bruteforce /login.php

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/09a61e0e-1240-4da3-adbb-eb98ae77b980)

`-z file,usernames.txt` loads the set of usernames, while `-z file,passwords.txt` loads the set of passwords.

--hs "Please enter the correct credentials" hides responses containing the string "Please enter the correct credentials", which is the message displayed for wrong login attempts.
-u specifies the target URL.
-d "username=FUZZ&password=FUZ2Z" gives the POST data format where FUZZ is replaced by usernames and FUZ2Z is replaced by passwords.

Thus upon running the command, we get the following output:

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/a57c4030-653f-475f-a603-84801206d79e)

which tells us the username(isaias) and the password(Happiness).
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/e5f2273d-5f78-4f26-b024-b12cc0329faf)


Thus upon entering the username and password, we get into the website, in which one of the mails contain the flag

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/acc362b6-3ded-43cd-83e2-191d3009135c)

### Flag
`THM{m3rrY4nt4rct1crAft$}`

##  A Christmas DOScovery: Tapes of Yule-tide Past

In this challenge, we need to restore a file `AC2023.BAK` from the Disk Operating System. The file is located in C:\TOOLS\BACKUP directory of the DOS. In order to access it, we can use the following commands:
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/3580d966-fa27-4b3a-8e2a-665e14e55ba5)
Here, we can also answer the question to how large the AC2023 file is which is `12,704` bytes
First, we use the `dir` command in order to see the directory and the files in the DOS. We can then navigate to the BACKUP directory using,

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/ccf37119-8dee-4fa5-bae5-ce7140317853)
Thus, we have reached the required directory. We need to run the command `BUMASTER.EXE C:\AC2023.BAK` in order to get the file, however, the file seems to be corrupted.

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/7ffa8f3a-6971-448a-867b-e040e392ce36)

Upon running `edit README.TXT`, to troubleshoot the problem,
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/434796a5-01ba-4acc-9fdb-aa88a00192b4)
We, therefore, need to alter the file's first 2 digits in order to be able to run the program. We also need to note that the first 2 digits must be of ASCII type.

Also, we can use the README.TXT, to determine the name of the program which is `BackupMaster3000`.
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/fbece4ca-c033-488f-9fce-dfff181fca36)

Upon running the command `EDIT C:\AC2023.BAK`, we get the following text editor.
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/537a6ae8-7939-43f2-a623-83eb5571afba). 

The challenge itself tells us that the magic bytes in hex format is '41 43'. We need to convert this to ASCII,
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/8220eea8-26d3-4cf0-a4db-392a92a06713)

Thus, AC has to be appended to the start of the text file, 
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/30e17a5a-b1f6-4f2f-ac6d-de6625c38cfe)

Now, we just need to run the command: BUMASTER.EXE C:\AC2023.BAK, which gives us the flag.

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/46c4d18e-c614-45b8-8605-ff3dcd0f1faa)

### Flag
THM{0LD_5CH00L_C00L_d00D}

## Memories of Christmas Past





































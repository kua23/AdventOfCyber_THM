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

## 5. A Christmas DOScovery: Tapes of Yule-tide Past

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

## 6. Memories of Christmas Past

This is a challenge based on buffer overflow. Thus, we are given a game in which we need to buy a star for the christmas tree. However, in order to buy the star we need 10000 coins, which is nearly impossible to get considering the fact that only one coin is received upon speaking to the computer in the game.

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/a567f279-645b-40cd-99a3-27cec583c5c0)

However, in the problem statement, they provide a hint saying that if we change our name to 'ScroogeRocks', we get additional coins. This is only possible because of a buffer overflow. As we can see in the debug menu
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/38c4af81-a73c-4ebf-9e6e-6e7b304743f1), the memory space for our player name is only upto 12 characters. However, when we change our name to one which has more than 12 characters, the data stored overflows into the next memory space which is in fact used to store the value of the coins collected. Thus, as the data is stored in hexadecimal notation, the number of coins increases.
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/a905c14b-00bf-45f3-9993-5763f15bab9f)

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/bcfc73b0-399e-4e39-914c-cb823849f6da)
In order to answer the first question, we first convert the given values from hexadecimal to ASCII. 
Here, 4f is equal to the letter 'O', 50 gives the letter 'P' and 53 gives the letter 'S'.

Thus, if we change our name to `AAAABBBBCCCCOOPS`, then the number of coins would be `1397772111`

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/a6de2d68-85ff-4029-88da-4aa6f441ce60)



Thus, if we provide a large enough name, we can easily obtain the 100000 dollars in order to decorate the christmas tree instead of clicking on the computer 10000 times which is tedious and honestly stupid.
Then, using the obtained coins we can buy the star. However, when we try to buy the star from the mouse, it refuses to give it to us, but instead gives us an item of an ID `e` instead of ID 'd'.
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/6326d62e-5218-4945-9476-3602e0346176)
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/cbdb30d6-c88d-4ab6-b5c2-1b23cac9f3dc)
However, after we get the item, in the inventory space we can see that an item with the ID `e` has appeared. Thus in order to get the star, if we change our name to one that is 46 letters long, with the last two letters ending with d, we can obtain the star.

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/f92aa9e9-ff1c-4e17-b541-e6b146fef336)
Then upon speaking with the tree, we get the flag.

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/80f9dd6d-55ba-477a-a79f-45116db2293a)

### Flag 
THM{mchoneybell_is_the_real_star}

## 7. ‘Tis the season for log chopping!

So, this is an interesting one. Here, we are given a set of log data and are given the task to determine and find out the malicious log data and retrieve it. We are also provided with additional small tasks in order to improve linux skills.

For the first question,
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/68cfdcda-b7d9-49ea-ad7d-003dbac71436)

In order to figure this out, we can just use

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/d3fa7404-a9fb-4ebd-9218-c967f718d339)

Thus, in this code,
`cut -d ' ' -f2 access.log | sort | uniq | nl`
`cut` is used to segregate the logs into different parts 
`-d ' '` is used to separate the logs based on the delimiter which in this case is ' '
`-f2` is used to tell the computer to just cut the string after the 2nd space as ' ' is the delimiter
`|` pipes the output of that part into the `sort` command
`sort` is used to sort the logs alphabetically
`uniq` is used to remove identical logs 
`nl` is used to count the number of lines

Then the answer returned is,

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/f7f48a57-d1ae-460b-9fae-1636da73b2a9)


Thus, the answer for this question is `9`

For the next question, ![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/40825d7e-ddfd-473a-89d7-566e1b95481c)

we use the code,
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/f4c34466-610a-4590-88dd-a336f8940e41)

`cut -d ' ' -f3 access.log | cut -d ':' -f1 | sort | uniq | nl`
which is almost the same as the previous code, except the fact that the first commands ouput, that is the domain and log part, is piped into the second command which is further cut due to there being a delimiter for ':'. This is done to further segment the log, so that only the domain name remains and the port is removed. This is then sorted, and filtered to finally get the answer which is `111`

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/da0406b2-0832-4675-b569-be949c4940f5)


For the next question,
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/f5c3ae44-5027-4795-b912-d07c53bd20a5)
something went wrong or I did not understand the question properly maybe. But the answer I finally got was correct.

Upon using this command `cut -d ' ' -f3,6 access.log| sort | uniq -c | sort | head`
This command gives the number of times the domain has been accessed the least
we got the status code for the second least accessed domain, not the first but it was fine.

Thus the answer is `503`


![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/87610de7-b677-4067-afb3-8e0872a3f75a)


For the next question, 
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/d8db57f3-1da8-4359-9c88-aa327472d7a9)
we can use,
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/9efd5d26-4d21-48e8-ab2c-d55596e23882)
`cut -d ' ' -f3 access.log | cut -d ':' -f1 | sort | uniq -c | sort | tail `
This basically first segments the logs such that only the website and port remains and then further segments it such that only the website remains. Then it sorts it and reqmoves duplicate log entries where `-c` gives a count of each entry. The next `sort` sorts the count in ascending order while the `tail ` command then displays the 10 most frequently visited websites.

Thus, upon noticing the most frequently visited websites, 
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/6ca4c78c-493c-4b06-ae14-aa609d31d896)
`frostlings.bigbadstash.thm` sure does stand out.

For the next question, 
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/f20aed38-307e-4fbe-b57e-d3dff7cccf07)
we cna use the following command:


![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/18e9806c-4d9b-4a8d-8f0f-39a42e4689fa)

`cut -d ' ' -f2,3 access.log | grep 'frostlings.bigbadstash.thm' | uniq`
This first segements the log such that only the IP address and the domain is visible and pipes it into the `grep` command which only filters those log entries which contain `frostlings.bigbadstash.thm`, then we remove the duplicate entries, to get the IP address.

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/00ffdf97-d2b1-4379-b3ef-5a069e726f00)
Thus, the answer is `10.10.185.225`

The next question is basically the same previous question
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/f4ac163f-ccaa-4889-a8ca-9d378315af85)
expect we use `uniq -c ` to count the number of times the domain has been accessed.

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/f1587d4c-b583-4d4f-837d-a3ff39cb0842)
Thus, the answer is `1581`

For the last answer, we  need to finally get the flag.

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/ca6c2b90-3c50-429c-b8e2-0ddfaf33536d)

When we run this command, `grep frostlings.bigbadstash.thm access.log | cut -d ' ' -f5 | head  `
we are basically telling the system to return the http url for all the logs which contain 'frostlings.bigbadstash.thm' and only show the last part of it.

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/93b00493-ccb5-4af5-8987-a7e22fe0978a)
The goodies part shows us that the data is mostly encoded and it does look like it is base64 encoded. So in order to decode this, we use 
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/db29ec51-a6ae-4fd3-b5cf-6c1307063adb)

in which one of the log entries contains the flag.
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/1a56490e-2157-42fa-a599-a5b495ec840b)

Thus, the flag is `THM{a_gift_for_you_awesome_analyst!}`

## 8.  Have a Holly, Jolly Byte!

In this challenge, we are using a software called FTK Imager to obtain information and history about evidence and analyse it.

For the first question, that is

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/dbd3cba7-06a9-4aac-a596-7354313d7147)

we need to first navigate to the `DO_NOT_OPEN` deleted folder, navigate to `secretchat.txt`, right-click it and export the files into a folder. Then navigating to the file, we get the answer. Thus, the answer is `mcgreedysecretc2.thm.`
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/222d3d71-eae8-4b91-87fc-8ab44c7f8130)


For the next question, in the same `DO_NOT_OPEN` folder, we need to navigate to `JuicyTomaTOY.zip` and export the files.
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/dff21d10-892e-448e-8d24-6040c73acf7e)

Then, upon going to the folder where the file is exported and then clicking on it, we realize that it contains a secret file named `JuicyTomaTOY.exe`

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/217f68fb-417f-4044-a865-ab479dc3433c)

For the next question, coming back to the ROOT file, to find the flag, we just need to check the hexdump file of the two deleted .png files, namely 'portrait.png' and 'wallpaper.png'. 
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/f87151a2-e532-42fe-92c0-f88417adaca5)

In the hexdump of 'portrait.png' on running the FIND tool and searching for `thm{`, we get the hidden flag
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/33f02b8f-93ab-49d6-b66a-c1f94e92c01a)
that is, `THM{byt3-L3vel_@n4Lys15}`

For the last question,
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/ffae465d-aad4-439c-bfb6-c230864d6387)

We need to navigate to
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/73aef688-6b36-4a72-9bae-2a78317ae95b)
to verify the disk image, which then gives us the SHA1 hash
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/bf168abd-0f08-4663-b191-af3a18d8fec8)
whose answer is `39f2dea6ffb43bf80d80f19d122076b3682773c2`

## 9. She sells C# shells by the C2shore

This challenge is basically based on reverse engineering the malware in order to obtain the solution. This is based on the .NET framework, which is sort of an intermediate between the high level C# language and the low level binary and assembly code. It is a Common Language Runtime which understands both of this.Thus it is easier to understand the malware, as it is written in source code and can be analysed at a level which is simpler to that of the assembly language. It also gives a basic foundation on C2 architecture which is basically Command and Control, in order to take control over the host's pc.

Thus coming to the questions, for the first one

 ![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/aa6120e4-e3a6-4507-9fe9-e833d6a765f7)

 If we navigate to the PostIt method, we do find the user agent which the malware used to connect to the C2 server.

 ![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/3802d921-fc98-44a3-9df3-cda075a7fbf9)
 Thus, the User Agent is given by `Mozilla/5.0 (Macintosh; Intel Mac OS X 14_0) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Safari/605.1.15`

 For the next question,

 ![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/4ec2348b-4fc4-4651-9180-c9544b8e1b30)

 This is very simple. The method used to submit the command execution output is `POST` after getting the instructions through the GETIT function.

 For the third question,

 ![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/48f479a7-c8d8-44b9-bdde-43fc40d22eec)

If we go into the Encryptor or the Decryptor function, we can see the key, which is assigned to the variable 'bytes'.

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/d03ba348-43e4-4fe1-b19d-1c8b72f2f26f)
Thus, the answer is `youcanthackthissupersecurec2keys`

To answer the next question, 
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/458080e3-32c3-4100-b019-f3d30cc5a604)
we just need to go to the main Program, and we can find the first URL used by the malware, which is given by `string url = str + "/reg";`

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/e28da78b-87be-4be9-ac36-e4622a84e2c5)

Thus, the first URL is `http://mcgreedysecretc2.thm/reg`

The next question too can be solved w ith referewnce to the main Program. 
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/7103e7fe-bfa6-4c83-9d08-e3be1506a49f)

The Sleeper function is used to put the malware program to sleep for 15000 milliseconds or `15` seconds.
![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/24e55741-c81a-4d38-af80-d8d92c72f80c)

For the next question,

![image](https://github.com/kua23/AdventOfCyber_THM/assets/61975172/0bdeb12d-5748-4744-81ee-cdc149fcfbe0)

If we just see the same main Program, 




 
















































































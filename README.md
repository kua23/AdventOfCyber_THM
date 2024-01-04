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

## Hydra is Coming to Town






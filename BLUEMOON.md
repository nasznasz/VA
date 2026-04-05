BLUEMOON :2021
Write Up
The VulnHub platform's "Bluemoon: 2021" Boot2Root machine walkthrough and penetration testing procedures are detailed in this report. Finding system vulnerabilities on the target machine, gaining initial access, and then performing privilege escalation to obtain root-level access are the main goals of this exercise.

 <img width="975" height="838" alt="image" src="https://github.com/user-attachments/assets/667a0c20-69c3-4b7a-bccb-0826a0fe28c8" />

Booting the Bluemoon target machine in Oracle VM VirtualBox.
 <img width="941" height="390" alt="image" src="https://github.com/user-attachments/assets/996a67bc-a531-400d-9d87-0130f90a0765" />

I started by running ifconfig on my Kali Linux machine to check my own network interface and subnet.

 <img width="941" height="390" alt="image" src="https://github.com/user-attachments/assets/55855ab8-0acd-41d0-bc33-313d412868bb" />

I ran nmap -sn 192.168.0.0/24 to sweep the local network. The scan identified the target machine running at the IP address 192.168.0.103.

 <img width="915" height="404" alt="image" src="https://github.com/user-attachments/assets/e5ef23bd-a1ac-4d12-97f5-05a1c9709f79" />

Executing sudo nmap -O 192.168.0.103 revealed that the target is running Linux and has three open ports: 21 (FTP), 22 (SSH), and 80 (HTTP)

 <img width="1013" height="845" alt="image" src="https://github.com/user-attachments/assets/9b6bcbb2-2a5a-4fab-974e-aaa8a59c2d06" />

Accessing the target's web server via a browser. I try navigating to http://192.168.0.103 on port 80 displayed a default webpage with the text "The Game Begins" and no immediate actionable clues.

<img width="975" height="298" alt="image" src="https://github.com/user-attachments/assets/c27c8916-4ea2-43ec-9144-2613c592f32c" />
 
After that, running an aggressive Nmap service scan. I used nmap -sV -p- -T4 192.168.0.103 to scan all ports and determine the exact versions of the running services.  

<img width="975" height="398" alt="image" src="https://github.com/user-attachments/assets/7c0f76d1-5a07-4a78-a2dd-cd65f0801d13" />
Using Gobuster to find hidden web directories. I use gobuster dir -u http://192.168.0.103 -w <wordlist> against the web server revealed a hidden directory.

<img width="975" height="464" alt="image" src="https://github.com/user-attachments/assets/9e698c96-88d2-4493-acee-6d40fbf0ca8b" />
Accessing the discovered hidden path in the browser. I navigating to 192.168.0.103/hidden_text showed a "Maintanance! Sorry For Delay" message. We can see there is a like embedded with “Thank you…” as we go inside that link that give as image of QR code now download it in our system.

 <img width="975" height="771" alt="image" src="https://github.com/user-attachments/assets/2e2ccf7a-d5c4-4a1b-8848-9e098f428f13" />
Now we move on to decode this QR code lets go to this website https://zxing.org/w/decode.jspx and upload the downloaded image to decode.

 <img width="975" height="681" alt="image" src="https://github.com/user-attachments/assets/65e592ea-7c37-4fdd-96ae-c7e914c45e33" />
I downloaded the QR code image from the target server and uploaded it to the ZXing Decoder Online tool to extract its hidden message.

 <img width="975" height="847" alt="image" src="https://github.com/user-attachments/assets/ec272466-20f1-4534-a49c-50b87b26be95" />
The decoder successfully translated the QR code into a bash script. The script contained hardcoded login credentials for the FTP service: Username userftp and Password ftpp@ssword.


 <img width="801" height="239" alt="image" src="https://github.com/user-attachments/assets/867347bb-bd03-4d7a-9503-843e40b2ee69" />
Using the terminal command ftp 192.168.0.103, I successfully logged into the FTP service using the extracted credentials (userftp / ftpp@ssword)

 <img width="975" height="238" alt="image" src="https://github.com/user-attachments/assets/978665ef-ef37-44d6-aac8-b92d81bea148" />
Running the ls command revealed two files: information.txt and p_lists.txt. I used the get information.txt command to download them to my local Kali machine for inspection.

<img width="975" height="615" alt="image" src="https://github.com/user-attachments/assets/f4fd892c-e450-4630-8d37-08c60186d33c" /> 
Using the cat command, I read information.txt, which was a message addressed to "robin" regarding a weak password. Reading p_lists.txt revealed a custom dictionary list of potential passwords.

 <img width="975" height="324" alt="image" src="https://github.com/user-attachments/assets/8fb564e9-40b7-48c6-8198-2529611aebfa" />
I run hydra -l robin -P p_lists.txt ssh://192.168.0.103 using the discovered username and custom wordlist. Hydra successfully found the valid password: k4rv3ndh4nh4ck3r.

 <img width="815" height="410" alt="image" src="https://github.com/user-attachments/assets/b4bce19c-ee5d-4538-94bc-882ff9120939" />

I connected to the machine using ssh robin@192.168.0.103 and the cracked password. Running whoami confirmed I successfully gained initial shell access as the user robin.

 <img width="698" height="339" alt="image" src="https://github.com/user-attachments/assets/a9d257b3-ea4f-4903-84fd-db5fc3ba2df0" />
Running ls -al showed a file named user1.txt. Using cat user1.txt revealed the first objective flag: Fl4g{u5er1r34ch3d5ucc355fully}.

 <img width="709" height="283" alt="image" src="https://github.com/user-attachments/assets/74a07438-16b8-41f1-996e-83a5b6e651f2" />
Inside robin's project folder, I found a bash script named feedback.sh. Using cat to read the script revealed that it takes user input and executes it without proper sanitization, making it vulnerable to command injection.

 <img width="975" height="490" alt="image" src="https://github.com/user-attachments/assets/ba2f2917-5f14-4c69-814c-5329aa3ab0aa" />
I ran ./feedback.sh and inputted /bin/bash when prompted. Because the script executes the input, it spawned a new shell as the user jerry. I navigated to jerry's home directory and read user2.txt to capture the second flag: Fl4g{Y0ur34ch3du53r25uc355ful1y}.

 <img width="975" height="620" alt="image" src="https://github.com/user-attachments/assets/1b0c20b3-e864-42d6-8d76-75ce9686425d" />
Checking id showed that jerry is part of the docker group. I exploited this by running docker run -v /:/mnt --rm -it alpine chroot /mnt sh, which mounted the host's root filesystem into a container and gave me a root shell. I navigated to /root and read root.txt to capture the final flag: Fl4g{r00t-H4ckTh3P14n3t0nc34g41n}.

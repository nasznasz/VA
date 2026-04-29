WEEK 7 – HYDRA (TryHackMe Lab Write-Up)
Introduction
This task focuses on using the Hydra tool to perform brute-force attacks on different services, specifically HTTP login and SSH. The objective is to obtain valid credentials and retrieve flags from the target machine.
 
 <img width="949" height="900" alt="image" src="https://github.com/user-attachments/assets/056267a9-eb42-4a0a-b9a3-c57fc587bb3f" />
Figure 1: successful run openvpn tryhackme.

1. Connecting to TryHackMe VPN
Before starting the attack, the TryHackMe VPN was activated to gain access to the target machine.

 <img width="880" height="866" alt="image" src="https://github.com/user-attachments/assets/10b57364-9328-4ecd-b210-f5984042396a" />

Figure 2: This screenshot shows the Hydra output where the correct login credentials were successfully identified. The tool tested multiple passwords until it found the valid combination.
2. Brute Force Attack on Web Login (HTTP)
To find valid login credentials for the web application, Hydra was used with the following command:
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.48.182.36 http-post-form "/login:username=^USER^&password=^PASS^:F=Your username or password" -V
This command attempts to brute-force the password using the rockyou.txt wordlist for the user molly.
Result
•	Username login: molly 
•	Password: sunshine 

 <img width="920" height="964" alt="image" src="https://github.com/user-attachments/assets/efab5a3c-946e-4054-b230-fc92617bd712" />

Figure 3: This screenshot shows the successful login to the web application using the discovered credentials. Access to the system was granted.
3. Accessing the Web Application
After obtaining the credentials, the target website was opened in a browser. The username and password were entered into the login page.

 
 <img width="921" height="927" alt="image" src="https://github.com/user-attachments/assets/2e81b0c3-de7c-43a1-b405-0872696819ef" />

Figure 4: This screenshot shows the first flag obtained from the web application after successful authentication.
4. Retrieving Flag 1
Once logged in, the first flag was located within the web interface.
FLAG1:
THM{2673a7dd116de68e85c48ec0b1f2612e}




<img width="900" height="873" alt="image" src="https://github.com/user-attachments/assets/aa28649d-0309-40e3-ae7c-b876f8ef42c1" />

5. Brute Force Attack on SSH
 
Figure 5: This screenshot shows Hydra successfully identifying the SSH password for the user “molly” after attempting multiple combinations.
Next, Hydra was used to perform a brute-force attack on the SSH service using the command:
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.48.182.36 -t 4 ssh
Result
Username: molly
Password: butterfly

<img width="938" height="904" alt="image" src="https://github.com/user-attachments/assets/c9e4d893-aa0b-4b7b-b79e-7cfa50cb3e48" />
 Figure 6: This screenshot shows a successful SSH connection to the target machine, indicating full access to the system shell.
6. Accessing the System via SSH
Using the discovered credentials, SSH access was established:
ssh molly@10.48.182.36
The password butterfly was used to log in.



 
 <img width="956" height="907" alt="image" src="https://github.com/user-attachments/assets/76f98ebb-cb00-4531-b4e8-68077cd18397" />
Figure 7: This screenshot shows the contents of the file “flag2.txt”, where the second flag was successfully retrieved.
7. Searching for Flag 2
After logging into the system, the directory contents were listed: “ls”.
A file named flag2.txt was found. The file was opened using: “cat flag2.txt”.

FLAG2: THM{c8eeb0468febbadea859baeb33b2541b}


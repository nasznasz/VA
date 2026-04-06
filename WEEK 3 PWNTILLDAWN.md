
WRITE UP WEEK 3 PWNTILLDAWN

Stage 1 : Reconnaissance 

 <img width="975" height="539" alt="image" src="https://github.com/user-attachments/assets/a9ecc052-afd8-4e0b-8b3e-78a31c57d7e3" />
The command “nmap -sn 10.150.150.0/24” was used to perform a “-sn”  "Ping Scan" across the network range. The perpose of this command is to tell which machines are powered on without performing a deep port scan yet, helping to find the starting IP. After get list of IP, we need to choose the first IP to further scan as the question needed.













Stage 2: Scanning

<img width="975" height="549" alt="image" src="https://github.com/user-attachments/assets/cdd69372-cbd8-4d90-a060-f8c874e50fb7" />
 
After choosing the first IP, we need to use command “sudo nmap -sV -sC -O 10.150.150.11” was executed to scan the specific target. This command is used for “-sV” detects service versions, “-sC” runs default scripts, and “-O” attempts to identify the Operating System. After performing this scan, we can see that there are multiple open ports, which possibly contain vulnerabilities. We found that the FTP and HTTP ports are open. We get ftp and hppt port was open. We can try to gather infomation from this two port.

 
<img width="975" height="904" alt="image" src="https://github.com/user-attachments/assets/645c4f1f-2e62-47d5-b1e1-bb16234e438a" />
After browsing this ip, it will open one website and have to sign in and find potensial weakness.

<img width="975" height="491" alt="image" src="https://github.com/user-attachments/assets/265c4c43-89e5-478a-acf9-0ecc274de7ef" /> 
After try use default username and password which is “admin”. We get this information that in this page have one png file.



















Stage 3 & 4: Gaining Access & Escalate Privileges
3.1. Web Exploitation
 
<img width="740" height="783" alt="image" src="https://github.com/user-attachments/assets/76802c72-0eb3-4dfe-b126-7f0765dc3b22" />
After that, we need to find hidden pages using the command gobuster dir -u http://10.150.150.11 -w /usr/share/wordlists/dirb/common.txt. We can see a list of hidden pages that can possibly be used to find the flag. For example, the hidden directory /admin/ contains sensitive files like manageusers.php which lists all user roles.

 
<img width="975" height="433" alt="image" src="https://github.com/user-attachments/assets/868113e9-06ab-4cd9-9f68-12b3ae3f7ca2" />
We can see after try to search 10.150.150.11/admin. It go to this page that show all admin can do.

 
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/c79631d3-e939-4f23-ba76-18619e95c0d6" />
This page shows all users and role in this website after we use directory manageuser.php.




3.2. System Exploitation (EternalBlue)
 
 <img width="748" height="260" alt="image" src="https://github.com/user-attachments/assets/bf4071ee-532a-4457-ae5f-8784cb991a35" />
Next, nmap command was used with the “smb-vuln-ms17-010” script to check for the EternalBlue vulnerability on port 445.

 
<img width="876" height="510" alt="image" src="https://github.com/user-attachments/assets/24ec1532-6c8b-43be-a40b-9bb90ecbcab4" />
Next, we used Metasploit to search for "eternal". It shows all modules that have the potential to gain access.

 <img width="796" height="573" alt="image" src="https://github.com/user-attachments/assets/e20a2138-b24c-4d73-9911-a7b8d9568e6e" />
I use to select 0 which is “exploit/windows/smb/ms17_010_eternalblue” module using command “use 0”. After that, I use “option” command to see all information in this module. 
 
<img width="806" height="520" alt="image" src="https://github.com/user-attachments/assets/9080e7fc-c99f-46f4-a6c8-0e0465bf743d" />
After changing the RHOSTS to 10.150.150.11 and setting up the LHOST, the EternalBlue exploit was run with the Metasploit framework. The exploit confirmed that the target was vulnerable and granted access to a Meterpreter session.

Stage 5: Maintain Access

 <img width="803" height="515" alt="image" src="https://github.com/user-attachments/assets/d7ad2b5b-1038-4b15-bab9-be427f814407" />
I needed to find proof that I had succeeded now that I was in charge, first I use root “shell”. After that, I used the “dir Desktop” command to look through the administrator's files. There was a file called “FLAG1.txt” on the desktop. I read the file by typing type Desktop\FLAG1.txt, which revealed the secret flag: PwnTillDawnAcademyIsAwesome!!!

Result FLAG
<img width="924" height="529" alt="image" src="https://github.com/user-attachments/assets/8e8c314f-508c-4e8b-b3ce-fe0a76af9421" />

 

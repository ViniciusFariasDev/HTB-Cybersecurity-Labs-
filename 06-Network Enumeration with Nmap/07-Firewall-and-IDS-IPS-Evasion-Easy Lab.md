At first, the question concerns which operating system and which name are on your machine.
So, we get the command nmap to search open ports
<img width="830" height="332" alt="image" src="https://github.com/user-attachments/assets/107373ff-2e68-4c7c-aab3-2714d71fef76" />

Later, We've to use the command sudo nmap -sV to get the OS name and their version in the port 22.
<img width="1043" height="361" alt="image" src="https://github.com/user-attachments/assets/6806f010-3952-401c-880e-2e7abf080f1d" />

And Later, We've to search the our target's DNS server version "sudo nmap -sV -p 53 10.129.2.48"
We've discover that port 53 is open 

Later, sudo nmap -sU -p 53 --script dns-nsid 10.129.2.48 to catch all infos 
<img width="847" height="261" alt="image" src="https://github.com/user-attachments/assets/54a94817-5ae5-4759-9780-39e35e20abd2" />

And finally, We've to use sudo nmap -sV -Pn -n -p 50000 --source-port 53 10.129.2.47 and connect with sudo ncat -nv -s 10.10.15.6 -p 53 10.129.2.47 50000
<img width="1052" height="595" alt="image" src="https://github.com/user-attachments/assets/65689856-2a5a-4d6d-b423-517cb2ae5e75" />

Started off with sqli on login page. Authentication bypass
	'or 1=1;--

Took me to search bar that allowed for more sql injections.
Used sqlmap for first time. Mapped entire database and pulled user hashes.
First used burp to catch request and save it as a txt, then used sqlmap.
	sqlmap -r request.txt -dbms=mysql --dump

From here I was able to use john to crack the hash which was saved in tmp files.
	john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-SHA256

Learned past john solves are in ~/.john/john.pot files


Using the cracked hash I could then ssh into the machine and check the sockets that open and hidden
	ss -tulpn

Found socket 10000 was being used and blocked by the firewall
Created ssh tunnel to expose the port so I could access on local machine
	ssh -L 10000:localhost:10000 <targetusername>@<targetIP>
	
Found Webmin server running. Logged in with already collected credentials.
Found CVS-2012-2982 and had to use python reverse shell.


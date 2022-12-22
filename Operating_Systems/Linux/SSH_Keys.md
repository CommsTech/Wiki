```
commstech@clustermgr:~$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/commstech/.ssh/id_rsa): /home/commstech/.ssh/ansible
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/commstech/.ssh/ansible
Your public key has been saved in /home/commstech/.ssh/ansible.pub
The key fingerprint is:
SHA256:XXXXXXXXXXXXXXXXXXXXXXXXX commstech@clustermgr
The key's randomart image is:
+---[RSA 4096]----+
|oo=*E   o.       |
|ooo+ ....        |
|  =o. + +        |
|o.......         |
|.++ + o S        |
|oB o.....        |
|+ B .  ..o  .    |
|.  ..........    |
|    o..o ++      |
+----[SHA256]-----+
commstech@clustermgr:~$ ls -l .ssh
```

Copy to another server
```
commstech@clustermgr:~$ ssh-copy-id -i /home/commstech/.ssh/ansible commstech@192.168.255.8
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/commstech/.ssh/ansible.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
commstech@192.168.255.8's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'commstech@192.168.255.8'"
and check to make sure that only the key(s) you wanted were added.

```

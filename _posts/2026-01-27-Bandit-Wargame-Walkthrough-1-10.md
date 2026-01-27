---
title: "Bandit Wargame 1-10 Walkthrough"
date: 2026-01-27
---


This is a minigame that tests your linux skill. I plan to solve it entirely and publich my solution here. Let's go.

I am writing this as I solve every level.

First things first, we need to ssh into level 0 using the provided credentials. 

```
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

It's going to ask for a password:
```
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit0@bandit.labs.overthewire.org's password: 
``` 
bandit0. boom. we're in. 

Running a very simple two commands to list the files and open the file we found gets us the password to the next level

```
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
Congratulations on your first steps into the bandit game!!
Please make sure you have read the rules at https://overthewire.org/rules/
If you are following a course, workshop, walkthrough or other educational activity,
please inform the instructor about the rules as well and encourage them to
contribute to the OverTheWire community so we can keep these games free!

The password you are looking for is: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

bandit0@bandit:~$ 
```

and We're in the next level.

Bandit1:
Again, using ls comes up with a file named "-". Since this file's name is only a special character, we need to open it in a special way; not really special but
writing the path before it.
```
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./-
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```
And we have the next level password.


Bandit2:
Again using "ls" shows us a file called "--spaces in this filename--". To open this kind of file there's two solutions. First you can use quotation marks as such:
```
bandit2@bandit:~$ cat "--spaces in this filename--"
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```
(As of 2026, this method is no longer working)


Or the more sophisticated option of adding a backslash before every space. This tells the terminal to ignore the (special character) after the slash.
```
bandit2@bandit:~$ cat ./--spaces\ in\ this\ filename-- 
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```
Bandit3:
Adding a dot before a file name in linux makes it so that the file isn't normally viewed by the usual ls. So to be more thorough we add the switches "-la"
A quick man search tells us that -l is for long listing format. It basically shows the permission the file has and the owner/users it belongs to, etc.
-a is for all. That means (you guessed it) showing all files. even the hidden ones.

```
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 Oct 14 09:26 .
drwxr-xr-x 3 root    root    4096 Oct 14 09:26 ..
-rw-r----- 1 bandit4 bandit3   33 Oct 14 09:26 ...Hiding-From-You
bandit3@bandit:~/inhere$ cat ./...Hiding-From-You 
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

We learned how to open files that begin with a special character. This should be easy for you.

Bandit4: Here we have a directory full of files and the password is in the only file that is human-readable. This shouldn't be hard. Just use file to see what file is readable.

```
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere/
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
bandit4@bandit:~/inhere$ cat ./-file00
�=
I�� ��V`n�5���ѳ��*�G^7cO�
bandit4@bandit:~/inhere$ file ./-file00
./-file00: data
```

Instead of checking every file for its type, we can use the star of the show: the asterisk (pun intended)
```
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: OpenPGP Public Key
./-file02: OpenPGP Public Key
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
bandit4@bandit:~/inhere$ cat ./-file07
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

Bandit5: This could be a long section on how to write the perfect command that searches for everything they've given us using grep/file/other convoluted commands.
But I am lazy, so I found out that I can only search for it by size. Maybe if they made it more difficult I would've worked harder. Here's the solution:
```
bandit5@bandit:~/inhere$ ls
maybehere00  maybehere02  maybehere04  maybehere06  maybehere08  maybehere10  maybehere12  maybehere14  maybehere16  maybehere18
maybehere01  maybehere03  maybehere05  maybehere07  maybehere09  maybehere11  maybehere13  maybehere15  maybehere17  maybehere19
bandit5@bandit:~/inhere$ find -size 1033c
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

Bandit6: If only I had shut my mouth. Now I have to work harder. sigh.
Writing the correct command gives a whole lot of permission denied messages:
```
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c -type f 
find: ‘/proc/tty/driver’: Permission denied
find: ‘/proc/1/task/1/fd’: Permission denied
find: ‘/proc/1/task/1/fdinfo’: Permission denied
find: ‘/proc/1/task/1/ns’: Permission denied
find: ‘/proc/1/fd’: Permission denied
find: ‘/proc/1/map_files’: Permission denied
...
```
A quick google search tells us to use 2>/dev/null to hide the error messages:
```
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c -type f 2>/dev/null
/var/lib/dpkg/info/bandit7.password 
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

Next level 

Bandit7: Simple grep command
```
bandit7@bandit:~$ grep "millionth" data.txt 
millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

Bandit8: An important note to successfully solve this one is in the man page of the command uniq: 
'uniq' does not detect repeated lines unless they are adjacent. You may want to sort the input first, or use 'sort -u' without 'uniq'.

Which brings us here: 
```
bandit8@bandit:~$ sort data.txt | uniq -u
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

Bandit9: Again, an important key to solve this one is in the man page of the command strings:
strings is mainly useful for determining the contents of non-text files:

```
bandit9@bandit:~$ strings data.txt | grep "==="
========== the
========== password
f\Z'========== is
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

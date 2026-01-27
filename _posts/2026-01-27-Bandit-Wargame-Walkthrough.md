---
title: "Bandit Wargame Walkthrough"
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
bandit3@bandit:~/inhere$ 
```

We learned how to open files that begin with a special character. This should be easy for you.

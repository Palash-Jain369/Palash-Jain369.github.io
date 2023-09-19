---
title: "Bandit"
date: 2023-09-11T10:47:20+05:30
draft: false
tags: [Linux, Hacking, Bandit, Overthewire]
image: https://github.com/Palash-Jain369/static/blob/main/Bandit_Thumbnail.png?raw=true
----



[**Bandit**](https://overthewire.org/wargames/bandit/) is a great resource to become familiar  with *Command Line* and Linux security fundamentals. In this wargame, you connect to a remote Linux machine over internet then solve various challenges through hints given at the [website](https://overthewire.org/wargames/bandit).

## Prerequisites
Although **Bandit** is considered beginner friendly- allowing the player to learn along as they play, learning a little bit about *Networking* especially- *OSI model, sockets, and overview of how networking protocols work*- in advance can make the experience better and more rewarding.

## Walkthrough
The focus will be on the approach and the concepts behind each level.

**Level 0** - 
```
ssh -l bandit0 -p 2220 bandit.labs.overthewire.org. 
```
```
ssh ssh://bandit0@bandit.labs.overthewire.org:2220
```
Use either one of the commands to connect to the remote server(computer), which is listening for incoming requests. After you enter the password, pay attention to the username in the terminal.

![Screenshot](https://github.com/Palash-Jain369/static/blob/main/Screenshot%20from%202023-09-13%2014-39-52.png?raw=true)



![](https://github.com/Palash-Jain369/static/blob/main/Screenshot%20from%202023-09-13%2014-44-28.png?raw=true)

After authenticating, the username changes from *'p'* to *'bandit0'*, and notice that even the title of the terminal changes. It is in the syntax 

`bandit0(username)@bandit(host/computer name).`

While *@host* is missing in the original title.  This is because terminal is now using the settings of the user 'bandit0' on bandit machine.

**Quick Note** -  Check out `man man`command . It will pull out the manual page of *man* command itself. No need to remember anything on the manual page, just keep in mind that this can make life a lot easier if navigating man pages seem too tedious.

![](https://github.com/Palash-Jain369/static/blob/main/man_termainal.png?raw=true)

![](https://github.com/Palash-Jain369/static/blob/main/man_page.png?raw=true)
**Level0 -->1 :** 

Start by `man ls`- the first command given in the hint. 

*Pro Tip* - 
If man pages are too overwhelming, try `ls --help`. It will give a brief description of the command and a few flags/options you can use. It will not open a separate page like in the case of a man page.

After gaining a basic understanding of the commands given in the hint, it will be clear that `ls` and `cat readme` commands will do the job. 

Be sure to save the passwords as you move to the next levels, or you will have to repeat the same levels every time you start afresh.

**Level 1-->2 :**
The next level is about *standard input* in Linux. ' - ' represents standard input, meaning the input taken from the user. If you checked out the man page for cat in previous level, you might have noticed that description of cat command is quite relevant for this level.

![](https://github.com/Palash-Jain369/static/blob/main/lvl1-2.png?raw=true)The problem is that '-' represents both file name and standard input, and cat is treating '-' as standard input instead of a file name. To force cat to treat '-' as a filename, enter the path of '-' file, meaning where it resides in file system 

```
cat ./-
```
' . ' represents the current working directory. `cat $(pwd)/-` will translate into same command as above. 'pwd' means Present Working Directory.

Now you may be wondering what is the purpose of just throwing back what user types at screen.  This is where redirection plays a role.
```
cat - > test.txt 
```
' > test.txt '  redirects the the output of cat command to a file named test.txt. So whatever the user typed will be stored in the file name test.txt instead of being thrown back at screen.

**Level 2-->3 :**
This level is based on how strings are represented in binary. Everything in computer memory is in the form of *0 and 1. For example 'A' is presented as *01000001*. Each character has a fixed amount of space allocated in the memory, so computer could access that many 0s and 1s and present it in form of characters.

However,  a string can be constituted from any number of characters. So '00000000' is considered as the end of a string in the memory, signalling computer to treat all characters before it as a singular unit.

The same applies for the bash shell. It treats space as end of the argument. So,  only spaces in `spaces in this filename` is considered as the argument for cat.

To get around this, put the file name in quotes.
```
cat "spaces in this filename"
```
Single quotes will also work but using double is advised. There is a slight difference in how bash interprets the argument placed inside the two.

**Level3-->4 :**
```
cd inhere

```
*Pro Tip* : Use `Tab` to autocomplete file/directory names.

`ls` command yields nothing. So some files must be hidden. Check man pages `man ls` or `ls --help`.  

![](https://github.com/Palash-Jain369/static/blob/main/lvl3-4.png?raw=true)Flags -a and -A seem quite interesting. It means that `ls` ignores files starting with ' . '  
```
ls -a
```
If ' . ' kind of rings a bell in your mind, you are onto something. we used `./-` command to ` cat ` out the contents of '-' file.  At that time, I said ' . ' represents current directory. So what these ' . '  ' .. ' directories doing here. Both of these are empty, and they exist in every directory of Linux filesystem. 

Try `cd ..` to see what happens.  

' . ' directory holds reference value of the said directory, kind of like a nameplate of house. If you tried the above command, you probably guessed that ' .. ' directory holds the reference to the said directory's parent in the file hierarchy.



## Level4-->5
At this point, you should go through man pages or `command --help` for the new commands given in the hints at each level.

![](https://github.com/Palash-Jain369/static/blob/main/lvl4-5.png?raw=true)
From the output of `file help`, we know that this command allows us to pass multiple filenames at the same time, making something like `file ./*` possible.  `*` is a 'regular expression', indicating all the files in the current directory. You can also give a path like `/dir1/dire2/*` instead of `$pwd/*`, or just two or more filenames like `file ./file1 ../file2`

![](https://github.com/Palash-Jain369/static/blob/main/lvl4-5_3.png?raw=true)
ASCII text is what we are looking for. ASCII is merely an agreed upon rules to represent binary in strings. Finding text files is a very useful in ethical hacking, since they can hold invaluable data. 

Don't consider the levels as simple puzzles, consider their utility against the backdrop of cybersecurity. 

## Level 5-->6

At this point, `--help` flag will not be enough. Man pages will necessary from hereon. In case of doubt, `man man` - man page of man command is great resource. 

```
find -size 1033c \! -executable
```
You can find this syntax by searching the manual page of find command. 
*(search for 'size' 'executable' 'invert' 'text' 'ASCII'....etc)*

'!' part is a bit tricky. ' ! ' is a special character that bash shell recognizes, so character \ is needed to make bash treat it as ordinary character. 

## Level 6-->7
A new command 'grep' appears in the hint section. Spend a bit more time in the manual page of grep command. You will be using this command a lot- both in this wargame and outside of it. Although grep command is not needed for this level, and `find / -user bandit7 -group bandit6 -size 33c` will do the job, do try the following command. 

```
find -user bandit7 -group bandit6 -size 33c 2>/dev/null| xargs cat
```
Try understanding how exactly it works.

*Hint : '2' = standard error; ' > ' = redirecting output; /dev/null = a file; | = unix piping ; xargs = command*

## Level 7-->8
```
grep millionth data.text
```
## Level 8-->9
```
sort data.txt | uniq -u 
```
## Level9-->10
```
strings data.txt | grep =
```
## Level 10-->11
As two systems communicate, they are merely receiving a continuous stream of bits- 0 for low current and 1 for high current. It turns out that there are some issues with interpretation of ASCII data by the receiving side, so BASE64 was introduced as a safe alternative to stop corruption of data over the network.

```
 base64 -d data.text
 ```
 
## Level11-->12
```
tr a-zA-Z n-za-mN-ZA-M data.txt
```
Pay attention to the asymmetric syntax a-z --->n-za-m. `Tr` only cares about checking that the number of characters on either side matches, so this syntax holds. Alternatively, you can try to split a-z into two parts as on the right side, it will work fine as well.





 
 


















 











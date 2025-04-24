
**Unit 1 Lab - Essential Tools Files, Redirects, and Permissions**

**Required Materials**

Putty or other connection tool

Lab Server

Root or sudo command access

**EXERCISES (Warmup to quickly run through your system and familiarize yourself)**

1.       mkdir lab_essentials

2.       cd lab_essentials

3.       ls

4.       touch testfile1

5.       ls

6.       touch testfile{2..10}

7.       ls

What does this do differently?

Can you figure out what the size of those files are in bytes? What command did you use?

8.       touch file.`hostname`

9.       touch file.`hostname`.`date +%F`

10.   touch file.`hostname`.`date +%F`.`date +%s`

11.   ls

What do each of these values mean? `man date` to figure those values out.

Try to set the following values in the file

year, just two digits

today's day of the month

Just the century

12.   date +%y

13.   date +%e

14.   date +%C

**LAB**

This lab is designed to help you get familiar with the basics of the systems you will be working on. Some of you will find that you know the basic material but the techniques here allow you to put it together in a more complex fashion.

It is recommended that you type these commands and do not copy and paste them. Word sometimes likes to format characters and they don’t always play nice with Linux.

**Working with files:**

1.       Creating empty files with touch

touch fruits.txt

ls –l fruits.txt

You will see that fruits.txt exists and is a 0 length (bytes) file

-rw-r--r--. 1 root root 0 Jun 22 07:59 fruits.txt

Take a look at those values and see if you can figure out what they mean.

man touch and see if it has any other useful features you might use. If you’ve ever used tiered storage think about access times and how to keep data hot/warm/cold. If you haven’t just look around for a bit.

rm –rf fruits.txt

ls –l fruits.txt

You will see that fruits.txt is gone.

2.       Creating files just by stuffing data in them

echo “grapes 5” > fruits.txt

cat fruits.txt

echo “apples 3” > fruits.txt

cat fruits.txt

echo “ “ > fruits.txt

echo “grapes 5” >> fruits.txt

cat fruits.txt

echo “apples 3” >> fruits.txt

cat fruits.txt

What is the difference between these two? Appending a file >> adds to the file whereas > just overwrites the file each write. Log files almost always are written with >>, we never > over those types of files.

3.       Creating file with vi or vim

It is highly recommended the user read vimtutor. To get vimtutor follow these steps:

sudo –i

yum –y install vim

vimtutor

There are about 36 short labs to show a user how to get around inside of vi. There are also cheat sheets around to help.

vi somefile.txt

type “i” to enter insert mode

Enter the following lines

grapes          5

apples           7

oranges        3

bananas       2

pears             6

pineapples  9

hit the “esc” key at the top left of your keyboard

Type “:wq”

Hit enter

cat somefile.txt

4.       Copying and moving files

cp somefile.txt backupfile.txt

ls

cat backupfile.txt

mv somefile.txt fruits.txt

ls

cat fruits.txt

Look at what happened in each of these scenarios. Can you explain the difference between cp and mv?

Read the manuals for cp and mv to see if there’s anything that may be useful to you. For most of us –r is tremendously useful option for moving directories.

5.       Searching/filtering through files

So maybe we only want to see certain values from a file, we can filter with a tool called grep

cat fruits.txt

cat fruits.txt | grep apple

cat fruits.txt | grep APPLE

read the manual for grep and see if you can cause it to ignore case.

See if you can figure out how to both ignore case and only find the word apple at the beginning of the line.

If you can’t, here’s the the answer. Try it:

cat fruits.txt | grep –i "^apple"

Can you figure out why that worked? What do you think the ^ does? Anchoring is a common term for this. See if you can find what anchors to the end of a string.

6.       Sorting files with sort

Let’s sort our file fruits.txt and look at what happens to the output and the original file

sort fruits.txt

cat fruits.txt

Did the sort output come out different than the cat output? Did sorting your file do anything to your original data?

So let’s sort our data again and figure out what this command does differently

sort –k 2 fruits.txt

You can of course man sort to figure it out, but –k refers to the “key” and can be useful for sorting by a specific column

But, if we cat fruits.txt we see we didn’t save anything we did. What if we wanted to save these outputs into a file. Could you do it?

If you couldn’t, here’s an answer:

sort fruits.txt > sort_by_alphabetical.txt

sort –k 2 fruits.txt > sort_by_price.txt

Cat both of those files out and verify their output

7.       Advanced sort practice

Consider the command

ps –aux

But that’s too long to probably see everything, so let’s use a command to filter just the top few lines

ps –aux | head

So now you can see the actual fields (keys) across the top that we could sort by

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND

So let’s say we wanted to sort by %MEM

ps –aux | sort –k 4 –n –r | head -10

read man to see why that works. Why do you suppose that it needs to be reversed to have the highest numbers at the top? What is the difference, if you can see any, between using the –n or not using it? You may have to use head -40 to figure that out, depending on your processes running.

Read man ps to figure out what other things you can see or sort by from the ps command. We will examine that command in detail in another lab.

**Working with redirection:**

The good thing is that you’ve already been redirecting information into files. The > and >> are useful for moving data into files. We have other functionality within redirects that can prove useful for putting data where we want it, or even not seeing the data.

1.       Catching the input of one command and feeding that into the input of another command

We’ve actually been doing this the entire time. “|” is the pipe operator and causes the output of one command to become the input of the second command.

cat fruits.txt | grep apple

This cats out the file, all of it, but then only shows the things that pass through the filter of grep.

We could continually add to these and make them longer and longer

cat fruits.txt | grep apple | sort | nl | awk ‘{print $2}’ | sort –r

pineapples

apples

cat fruits.txt | grep apple | sort | nl | awk '{print $3}' | sort -r

9

7

cat fruits.txt | grep apple | sort | nl | awk '{print $1}' | sort -r

2

1

Take these apart by pulling the end pipe and command off to see what is actually happening:

cat fruits.txt | grep apple | sort | nl | awk '{print $1}' | sort -r

2

1

cat fruits.txt | grep apple | sort | nl | awk '{print $1}'

1

2

 cat fruits.txt | grep apple | sort | nl

     1  apples  7

     2  pineapples      9

cat fruits.txt | grep apple | sort

apples  7

pineapples      9

cat fruits.txt | grep apple

apples  7

pineapples      9

See if you can figure out what each of those commands do.

Read the manual `man command` for any command you don’t recognize.

Use something you learned to affect the output.

2.       Throwing the output into a file

We’ve already used > and >> to throw data into a file but when we redirect like that we are catching it before it comes to the screen. There is another tool that is useful for catching data and also showing it to us, that is tee.

date

comes to the screen

date > datefile 

redirects and creates a file datefile with the value

date | tee –a datefile

will come to screen, redirect to the file.

Do a quick man on tee to see what the –a does. Try it without that value. Can you see any other useful options in there for tee?

3.       Ignoring pesky errors or tossing out unwanted output

Sometimes we don’t care when something errs out. We just want to see that it’s working or not. If you’re wanting to filter out errors (2) in the standarderr, you can do this

ls fruit.txt

You should see normal output

ls fruity.txt

You should see an error unless you made this file

ls fruity.txt 2> /dev/null

You should no longer see the error.

But, sometimes you do care how well your script runs against 100 servers, or you’re testing and want to see those errors. You can redirect that to a file, just as easy

ls fruity.txt >> error.log

cat error.log

You’ll see the error. If you want it see it a few times do the error line to see it happen.

In one of our later labs we’re going to look at stressing our systems out. For this, we’ll use a command that basically just causes the system to burn cpu cycles creating random numbers, zipping up the output and then throwing it all away. Here’s a preview of that command so you can play with it.

May have to yum –y install bzip2 for this next one to work.

time dd if=/dev/urandom bs=1024k count=20 | bzip2 -9 >> /dev/null

Use “crtl + c” to break if you use that and it becomes too long or your system is under too much load. The only numbers you can play with there are the 1024k and the count. Other numbers should be only changed if you use man to read about them first.

4.       This is the “poor man’s” answer file. Something we used to do when we needed to answer some values into a script or installer. This is still very accurate and still works, but might be a bit advanced with a lot of advanced topics in here. Try it if you’d like but don’t worry if you don’t get this on the first lab.

vi testscript.sh

hit “i” to enter insert mode

add the following lines:

#!/bin/bash

read value

echo "The first value is $value"

read value

echo "The second value is $value"

read value

echo "The third value is $value"

read value

echo "The fourth value is $value"

hit “esc” key

type in :wq

hit enter

chmod 755 testscript.sh

Now type in this (don’t type in the > those will just be there in your shell):

[xgqa6cha@N01APL4244 ~]$ echo "yes

> no

> 10

> why" | ./testscript.sh

yes

no

10

why

What happened here is that we read the input from command line and gave it, in order to the script to read and then output. This is something we do if we know an installer wants certain values throughout it, but we don’t want to sit there and type them in, or we’re doing it across 100 servers quickly, or all kinds of reasons. It’s just a quick and dirty input “hack” that counts as a redirect.

**Working with permissions:**

Permissions have to do with who can or cannot access (read), edit (write), or execute (xecute)files.

Permissions look like this:

ls –l

|   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|
|Permission|Number of hard links|UID Owner|Group Owner|Size in bytes|Creation<br><br>Month|Creation<br><br>Day|Creation<br><br>Time H:M|Name of File|
|-rw-r--r--.|1|Root|root|58|Jun|22|08:52|datefile|

The primary permissions commands we’re going to use are going to be chmod (access) and chown (ownership).

A quick rundown of how permissions break out. I will explain this on the call.

![](data:image/png;base64,/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAoHBwkHBgoJCAkLCwoMDxkQDw4ODx4WFxIZJCAmJSMgIyIoLTkwKCo2KyIjMkQyNjs9QEBAJjBGS0U+Sjk/QD3/wAALCADVAZcBAREA/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/9oACAEBAAA/APZqKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKK4CG717VtZ8SGPxKdNttMufLiVrWFo1XYGyxYZx+NJB4w1W48MeFNfmxbw3d2tvqEQQbWVyyK+SMqMgNwf4u9T+MvFt/ovi3R7W0YCxTbLqWVB/dySCJOSMjB3HjHStfU9UvD430bSLGXZE0M13ejaCTGAFQcjjLnt6Vgt41vvD0WraTq7/a9agkA035FQ3yyHEWAMDIPDYx0rs9HgvbfSLWLVLr7VerGPPm2qoZ+pwAAMDoOOgq7RRRRXE3lzrWp+PtR0qz12TTbS1s4plEdtFJlmJySXUnt61nDxdrcngG/1ZJ45J9J1IxSTwxLsvYI3UMyg5ABDHkf3eDV/wCIvirUNDsNNfQmVpZXa5k+UMGto13P1BxnK81peI9buVk8P2ujzhJdUvE+cKG/0dVLyEZyPugD8aoXni6Xwnr+qW3iO48yykha806XYqkgfeg4AywOMZ5IPJrd8Lf2s+hxT67Jm9uCZmiCqogVuVjGBztGOTk5zzWxRRRRXHeIrzV5/HGl6Np2qyadBPaSzSNHBHISVIx99TVC11zX7jTfFunw6hHdXujqv2XUIoUHmsULlCvK7hjaeO/QVa8ReK75vh7YajoTBdS1TyUtvlDbXYbm4II4Aan6p4rupvhzY6npcipqWpiCG3O0MFmkYA8Hg4+b8qm1TX7rwv4qgOsXYbQr+LYkroqi2nQZIJA6OATznnpgVc8H3+pazYz6tfsyW17JvsbYoo8qAcKSQMkt945J7YxXQ0UUUUUUUUUUUUUUUUUUUUUV5faeDNL8V614zW+tlN0L0JBcZIaI+WCCD9cGtSAy+NPhNdWs6bNRihe3kQAArcQnjp0yyqeOmazdDt38f+F/E2pyoVk1OGO2gDD7piiBBHt5rN+VaHw4vZfElxf+JbpSpeCCyQuMY8tA0v4GRz+VZl9aah4vubvxhpbEHSn26PHgYuVjJ81j6h+VH0r0LRNXt9e0W11OzJMNym8A9VPQg+4II/Cr9FFFFed3XhvS/EnxR1q31ezS4jXToNm4kFCSRkEdDV/wdF9t8Nar4V1LmTTnksJDtA3wsCY3x7qf0zWL8P0l8R3NxFqcbY0nTBo0isMhnLMJCP8AgKR/nS/Dr7TqeuQx3qsT4Zs304kjgzGQrkf9s40/OrGv6bP8RNavYrOcwWehgpbTLjEl9wev91MAEepNdZ4S1/8A4SLQY7qWPybuNjBdwnrFMvDL/X6EVtUUUUVwXifR7HW/idotpqdqlzbHT52KOOMhhg1Z8ExroGsax4UK4gtmF3ZbuS0EnUZ77WBGTzWD4Rgl/wCEzh8Ouji38Ny3cyE9GWQgQ/jtkf8AKm+HrWVvG0Xhh0P2XQb65vxx8uxwDAPqDK5/CtrxVbDxzr48LI7Jp9mguNRlTGQ5B8qMHsf4j7AVp+B9WuLvTptL1PA1XSHFtc9t4x8kg9mUZ/OunoooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooorN1zxDpnhqxF5rF0ttAXCBirMSx5wAoJPQ9u1aCMHRXU5VhkH2rD1LxtoGkXj2t9qKRyxkCTbG7rET03soIX8SK3I5EljWSNldHAZWU5BB6EGs/V/EOl6D5H9p3kdu1w4jiU5LOxOOAMnv17VpUUUUUVT1XVbPRNOlv9SnWC1hALyEE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)

Let’s examine some permissions and see if we can’t figure out what permissions are allowed

ls -ld /root/

drwx------. 5 root root 4096 Jun 22 09:11 /root/

The first character lets you know if the file is a directory, file, or link. In this case we are looking at my home directory.

rwx – for UID (me). What permissions do I have?

--- - For group. Who are they? What can my group do?

--- - For everyone else. What can everyone else do?

Go find some other interesting files or directories and see what you see there. Can you identify their characteristics and permissions?
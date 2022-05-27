![GitHub Light](https://github.com/github-light.png#gh-dark-mode-only)

# **Final \[PicoCTF Challenges\]**

Description: “Pick 10 (TEN) challenges, solve them and do a write-up with how you solved it in detail. Include screenshots if necessary.”

For these challenges I thought I’d push myself and focus on web exploitation but also test some general skills, so this’ll be a 50/50 split.

# **WEB EXPLOITATION**
## **GET aHEAD \[20 POINTS\]**
Description: “Find the flag being held on this server to get ahead of the competition.”
My first approach was to inspect the page which shows just a simple html file to match the simple webpage.

![get ahead](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B1%5D%20get%20ahead.png)

But since the file has the .php extension this is more of a script that is typically executed on servers. And with the two options provided to select red/blue, I notice that there’s a method variable with GET for red and POST for blue which confirms information is being sent to a server. GET and POST are HTTP requests and with the title of this being ‘GET aHEAD’ I think that’s a very solid hint to essentially create our own option to send a HEAD request to the server. 

I’m not sure of any other way to do this than using curl.

![curl flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B2%5D%20curl%20flag.jpg)

\[The -I flag shows the header\]

Since the flag was in the packet’s header we can view it with the previous command and obtain the flag.

## **COOKIES \[40 POINTS\]**
Description: “Who doesn’t love cookies? Try to figure out the best one.”

So I went to see if there were any cookies saved, and there was one with the name ‘name’ and a value of -1 \[so name = -1\]. I know sometimes the value of a cookie can be associated with specific users such as admin accounts so I thought I would play around with the values.

After changing the value to 0 and refreshing the page, I got a new display:

![snickerdoodle](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B3%5D%20snickerdoodle.jpg)

Since we’re guessing the best cookie and each cookie value is connected to a type of cookie, it’d make sense that a value would equal the flag we’re after. We can run curl while the value increments by 1 until the flag is found using a python script, orrrrr I can guess and refresh.

![cookie val](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B4%5D%20cookie%20value%20flag.jpg)

\[I guessed and refreshed. The cookie value was 18.\]

## **INSP3CT0R \[50 POINTS\]**
Description: “Kishor Balan tipped us off that the following code may need inspection: https://jupiter.challenges.picoctf.org/problem/9670/”

A hint seems to be to inspect the webpage, so I looked at just the html code:

![html flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B5%5D%20html%20flag.jpg)

And apparently in the comments holds part one of three for the flag. So let’s look at the other files beginning with the .css file:

![css flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B6%5D%20css%20flag.jpg)

There’s part two of the flag! When we look at the “How” tab of the website the creator states they used HTML, CSS, and JS. Let’s look at the .js file next:

![js flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B7%5D%20js%20flag.jpg)

We found the entire flag and now we’re done.
picoCTF{tru3_d3t3ct1ve_0r_ju5t_lucky?2e7b23e3}

## **SCAVENGER HUNT \[50 POINTS\]**
Description: “There is some interesting information hidden around this site <link>. Can you find it?”

Again I looked at the webpage files used and found part 1 and 2 of the flag.
Html file:

![html flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B8%5D%20html%20flag.jpg)

mycss.css:

![css flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B9%5D%20css%20flag.jpg)

The other file is a javascript file with the following comment:

![js comment](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B10%5D%20js%20comment.jpg)

Google has crawlers that index websites to provide them on google searches. A way to opt-out of being indexed is by including a robots.txt file so if there’s a file with this name on the server where then we should be able to add the name to the URL:

![robots flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B11%5D%20robots%20flag.jpg)

Now we have part three and a clue! Apache has configuration files with a common name of .htaccess which allows for configuration changes for directories. So let’s see if this webpage has an .htaccess file available:

![apache flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B12%5D%20apache%20flag.jpg)

And we found part 4 (really thought this would be the last one but that’s ok). Now this hint is about using a Mac and a Store? I’m very unfamiliar with Apple products so let’s head to Google. I figured I’d go with the theme here which is files and searched up “mac store file” and got a hit on .DS_Store files. It’s a hidden file that’s created anytime you look into a folder and stores information on the files inside and any surrounding files/directories.
So let’s add .DS_Store to the URL:

![mac flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B13%5D%20mac%20flag.jpg)

We’re finally done and have found the entire flag!
picoCTF{th4ts_4_l0t_0f_pl4c3s_2_lO0k_74cceb07}

## **LOGON \[100 POINTS\]**
Description: “The factory is hiding things from all of its users. Can you login as Joe and find what they've been looking at? https://jupiter.challenges.picoctf.org/problem/44573/ (link) or http://jupiter.challenges.picoctf.org:44573”

We want to know what this Joe fella has been looking at so a cookie seems to be the place to look, but first let’s try to login to see if we get anything. Apparently, their password is super secure so let’s just look to see if there are any cookies or cache.

In the cookie section there is a username and password variable but also an admin variable set to false:

![logon cookie](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B14%5D%20logon%20cookie.jpg)

I wonder if we can just change it to true to gain access:

![logon flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5B15%5D%20logon%20flag.jpg)

Once we refreshed the page the flag was given.

# **MISCELLANEOUS**
## **OBEDIENT CAT \[5 POINTS\]**
Description: “This file has a flag in plain sight (aka "in-the-clear"). Download flag.”
We open a file and find the flag:

![cat](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5Bobedient%20cat%5D%20flag%20file.jpg)

\[Note: I’m sure they wanted us to use the cat command\]
## **PYTHON WRANGLING \[10 POINTS\]**
Description: “Python scripts are invoked kind of like programs in the Terminal... Can you run this Python script using this password to get the flag?”

First download the python script, the password, and flag file. When we run the script it prompts us to use the -e or -d flag with a file name. These flags might represent encode and decode so let’s try running the script with the -d flag and use the flag file:

![flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5Bpython%20wr%5D%20flag.jpg)

After inputting the password we were given we get the flag.

## **WAVE A FLAG \[10 POINTS\]**
Description: “Can you invoke help flags for a tool or binary? This program has extraordinarily helpful information...”

Again, just download the file and run it, but we see we have to add the -h flag and there we go!

![warm](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5Bwave%20flag%5D%20warm.jpg)

\[Note: don’t forget to add the execute permission if you can’t run the file.\]

## **STATIC AIN’T ALWAYS NOISE \[20 POINTS\]**
Description: “Can you look at the data in this binary: static? This BASH script might help!”

After downloading the bash script and binary file we can run the script with the file:

![ltdis](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5Bstatic%5D%20ltdis.jpg)

So let’s open the strings file and use grep to help us out:

![ltdis flag](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5Bstatic%5D%20flag.jpg)

## **TAB, TAB, ATTACK \[20 POINTS\]**
Description: “Using tabcomplete in the Terminal will add years to your life, esp. when dealing with long rambling directory structures and filenames: Addadshashanammu.zip”

Download the file and unzip it. This adds a directory and as we traverse through the layered directories (seven to be exact) we find an executable. To get the flag we simply run the file:

![tab](https://github.com/Tarroyo-rgb/CSCI_17_SP22/blob/main/Images/FINAL/%5Btab%5D%20flag.jpg)

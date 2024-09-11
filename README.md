# MISCELLANEOUS CHALLENGE
## THM_Simple-CTF

**Description**
A writeup of the Simple CTF room on Tryhackme, Beginner level ctf

## Walkthrough

## Question 1
How many services are running under port 1000?

#### Method
    sudo nmap -sS -p- -T4 x.x.x.x -vv

    -sS to run a Syn/Stealth scan
    -p- to scan all 65545 ports
    -T4 for an agressive and noisy scan
    -vv for very verbose so we can monitor progress of the scan

#### Answer
2 

![alt text](/images/q1.png)

## Question 2
What is running on the higher port?

#### Method
Allowing the last command to run allowed us to discover an open port

![alt text](/images/q2A.png)

Lets find out a bit more information about this port

    sudo nmap -sS -sV -p21,80,2222 -T4 x.x.x.x -vv

    -sV to run a version scan to see what each port is doing

![alt text](/images/q2B.png)

So we can see that SSH is running on this higher port

#### Answer
ssh

## Question 3
What's the CVE you're using against the application?

#### Method
From the nmap scans we have found that anonymous logins are allowed on this service. So thats our next task.

![alt text](/images/q3A.png)

    ftp x.x.x.x
    ls

![alt text](/images/q3B.png)

Ok so theres a folder called 'pub' on the directory, lets cd into there and look around

![alt text](/images/q3C.png)

Ah! A file named 'ForMitch.txt is seen. Lets use get to download the file to our machine and then read its contents. 

    get ForMitch.txt
    exit
    cat ForMitch.txt

![alt text](/images/q3D.png)

So with this .txt we can assume that Mitch has stored his password somewhere that still exists, our goal is to find it. We can turn our attention to SSH. We can't brute force the password for Mitch yet. We know there is a webserver running on port 80. This is a valid way of gaining some extra details about the machines IP. So lets navigate to this by simply putting our running machines IP into the browser and search. This shows that on port 80 there is a live default Apache page, so not really anything useful here.
We switch techniques and use a tool name Gobuster. Gobuster is a tool used to brute-force hidden web objects on a target using a dictionary attack. To download the tool and use it:

    sudo apt install gobuster
    gobuster dir -u http://x.x.x.x/ -w /usr/share/wordlists/rockyou.txt

Gobuster will start and some results will return, so we wait a couple of minutes to see the full outcome. The most interesting of the entries is /simple with 301 redirect. So we just type into our browser x.x.x.x/simple to see whats going on. 

![alt text](/images/q3E.png)

So the site tells us CMS is secure... Lets try to bypass the sites security. So we know there shoulf be a login page at /admin where one could make changes to the CMS. The version specs are also listed on this site as 'CMS Made Simple 2.2.8'. So we have the service version so we can check out ExploitDB for any existing vulnerabilities and exploits. You can either use a built in tool to search or just the web version, I just checked the web version. So one exists!

![alt text](/images/q3F.png)


#### Answer
CVE-2019-9053

## Question 4
To what kind of vulnerability is the application vulnerable?
#### Method
So from the previous question we have already gotten this information. 

#### Answer
sqli

## Question 5
What's the password?
#### Method
Download the exploit from ExploitDB. Or download from Terminal using

    searchsploit -m 46635

To use the script run:

    python 46635.py -u http://x.x.x.x/simple/ --crack -w /usr/share/wordlists/rockyou.txt

However! I did run into problems as the script in running Python 2 but we are running Python 3. So I added enclosed () to all print statements to fix this. So after making tweaks to the Python script, run it again. Wait a bit while it finds the details. 

![alt text](/images/q4.png)

#### Answer
secret

## Question 6
Where can you login with the details obtained?
#### Method
Common knowledge

#### Answer
ssh

## Question 7
What's the user flag?
#### Method
Log in using all the information we've discovered up to this point.

    ssh -p 2222 mitch@x.x.x.x

After gaining access have a look around using basic linux commands. Find the file in the /mitch directory and read it to gain the flag.

![alt text](/images/q7.png)

#### Answer
G00d j0b, keep up!

## Question 8
Is there any other user in the home directory? What's its name?
#### Method
So cding into the /home directory shows us there is another user other than mitch - sunbath

#### Answer
sunbath

## Question 9
What can you leverage to spawn a privileged shell?
#### Method
As we can see above Mitch user has accessed vim editor with sudo. We can run vim as sudo. We can escalate our privileges to root:

    sudo vim -c ':!/bin/bash'

![alt text](/images/q9A.png)

So we have access! We have also finally gotten into /sunbath! Having a look around theres a lot more info here. 

#### Answer
vim

## Question 10
Is there any other user in the home directory? What's its name?
#### Method
So cding into the /home directory shows us there is another user other than mitch - sunbath

#### Answer
sunbath
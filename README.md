# MISCELLANEOUS CHALLENGE
## THM_Simple-CTF

**Description**
A writeup of the Simple CTF room on Tryhackme, Beginner level ctf

## Walkthrough

### Question 1
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

### Question 2
What is running on the higher port?

#### Method
Allowing the last command to run allowed us to discover an open port

![alt text](/images/q2A.png)

Lets find out a bit more information about this port

    sudo nmap -sS -sV -p21,80,2222 -T4 x.x.x.x -vv

![alt text](/images/q2B.png)

So we can see that SSH is running on this higher port

#### Answer
ssh

### Question 3
What's the CVE you're using against the application?

#### Method
sudo nmap -sS -p- -T4 x.x.x.x -vv

    -sS to run a Syn/Stealth scan
    -p- to scan all 65545 ports
    -T4 for an agressive and noisy scan
    -vv for very verbose so we can monitor progress of the scan

#### Answer
2 
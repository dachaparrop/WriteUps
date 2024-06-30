# [Fare Evasion] <ChallengeName>
## Author: Louis <AuthorName>
### Points: 138 <points>

### Context

SIGPwny Transit Authority needs your fares, but the system is acting a tad odd. We'll let you sign your tickets this time!

https://fare-evasion.chal.uiuc.tf/


### Solution

The website seems to be a tutorial for downloading or obtain an extension to support github web pages

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/82f92d3a-fb17-4d20-a837-3f5d2aba8019)

We see a button gets us the wrong flag:

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/e0b1de4c-07d1-4733-82cd-989dfae968f0)

Doing hovering and seeing the source code on the page, we see that every extension have the same source code:

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/d344da46-f042-47a8-a16d-b9832e50d0af)

We don't see anything relevant, so we intercept the request in Burpsuit when clicking the button:

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/64ee3def-0c5d-42bd-b642-85e013795d0b)

The request is doing a GET to a "DUMMY.txt" resource, and the button says "Fetch FLAG.txt", we could change the name of the resource:

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/44b13360-dc05-4669-ad98-140fe75cd9cc)

And we got the flag

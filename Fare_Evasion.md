# [Fare Evasion] <ChallengeName>
## Author: Louis <AuthorName>
### Points: 138 <points>

### Context

SIGPwny Transit Authority needs your fares, but the system is acting a tad odd. We'll let you sign your tickets this time!

https://fare-evasion.chal.uiuc.tf/


### Solution

The website is related to a transit system where you can enter as a passenger or as a conductor, but if we click in the "Im a passenger" button, we see this alert:

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/d281b2db-f625-4067-b1d1-584b99e8f162)

There are a lot of strange characters and a message.

So we must try to do a bypass with the conductor rol to acces the website, now analysing the code source in the page, we see this comment:

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/a1d3d9b2-0bde-4fb6-81f5-e3d89afe8659)

With wappalyzer we can identify that the website is using Python as a programming language, and that is doing a wuery to an sqlite database.

Also, we can see that is requering the "headerKid", that can be a part of a JWT, so we have to intercept a request:



And we got the flag

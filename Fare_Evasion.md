# [Fare Evasion] <ChallengeName>
## Author: Louis <AuthorName>
### Points: 138 <points>

### Context

SIGPwny Transit Authority needs your fares, but the system is acting a tad odd. We'll let you sign your tickets this time!

https://fare-evasion.chal.uiuc.tf/


### Solution

The website is related to a transit system where you can enter as a passenger or as a conductor, but if we click in the "Im a passenger" button, we see this alert:

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/d281b2db-f625-4067-b1d1-584b99e8f162)

There are a lot of strange characters and a message telling us a "secret".

So we must try to do a bypass with the conductor role to acces the website, now analysing the code source in the page, we see this comment:

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/a1d3d9b2-0bde-4fb6-81f5-e3d89afe8659)

With wappalyzer we can identify that the website is using Python as a programming language, and that is doing a query to an sqlite database using a POST method in the endpoint /pay.

Also, we can see that is requering the "headerKid", that can be a part of a JWT, so we have to intercept a request:

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/f99c16ac-21a1-4491-9048-fbd1480a5094)

We confirm that they are using a Key id header in the json web token, so the secret in the message can be the one that the website gave us "a_boring_passenger_signing_key_?"

If we modify the payload in the JWT without the secret, It gives us this error:

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/69a9c5f4-8eba-4f32-886c-c0982d6f8021)

But, if we put the secret, the error is the same as before, so we confirm that this is the secret in the JWT for the passenger role

Doing a searching, there are a lot of websites that shows vulnerabilites with injections in this particular header, related to the clues that are in the source code and with the secret of the JWT, we can suppose that we have to do an ijection in the kid header.

Firstly, we try with different characters that can shows us a different error message, to discover if the injection is done in python or in the sqlite database

![image](https://github.com/dachaparrop/WriteUps/assets/112051369/bb83b87b-f79d-4f21-82da-c338435816ce)

This error is caused in python, and trying more kind of payloads in the kid header, we don't get anything interesting.

So, looking again in the source code, 
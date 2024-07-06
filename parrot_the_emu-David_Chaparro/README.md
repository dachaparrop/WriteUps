# [Beginner] parrot the emu <ChallengeName>
## Author: David Chaparro <AuthorName>
### Points: 100<points>

### Context

It is so nice to hear Parrot the Emu talk back

https://web-parrot-the-emu-4c2d0c693847.2024.ductf.dev 


### Solution

#### Requirements

+ Python
+ Flask
+ SSTI

#### Step 1
The link shows us a web application where we can talk and the server answers the same thing, so, we can try different kind of attacks, such as XSS, SSTI, SQLI or another type of injection

![01](./assets/3.png)  


#### Step 2

With wappalyzer, we can see that the page is using python and is made in Flask, so after trying some XSS payloads, we try a SSTI attack and got this:

![01](./assets/4.png)

#### Step 3

Now, looking in the files from the zip that the challenge gave us, we realize that the flag is in the web directory of the page:

![01](./assets/5.png)

#### Step 4

So, we need to do a code injection that can gives us back the content of the flag file.

Looking up online, we found a page telling us that there is a subclass that allows us to read and write files in the web server, but in order to do that, we have to know where is the class.

https://payatu.com/blog/server-side-template-injectionssti/

We use the `__subclasses__()` method to find the correct classes that content the class we are looking for, and we use `__mro__` (method resolution object) to climb the inheritance tree and acces the classes.

These results are always returning us a list of susbclasses, we have find the position of the class that we are looking for, to put the next position in the inheritance tree.

The payload:

```python
{{ ''.__class__.__mro__[1].__subclasses__() }}
```

give us a large list of subclasses, where we find the first class we need:

![01](./assets/6.png)

#### Step 5

This class is in the position 92 in the list, we can select it to confirm:

![01](./assets/7.png)

#### Step 6

Now, we repeat the same process twice until we reach the class we have looking for:

![01](./assets/8.png)

We found it!! Now we only have to apply the `write()` method and specify the path of the flag:

#### Step 7

Using a relative path, and this payload: `{{ ''.__class__.__mro__[1].__subclasses__()[92].__subclasses__()[0].__subclasses__()[0]('flag').read() }}`

![01](./assets/9.png)

We got it!

##### Code alt1

```javascript
console.log("Example use alt1");
```

##### Code alt2

Source code available on [script](./scripts/code.js)

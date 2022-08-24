<H1> Building A Defensive Security Monitoring Environment Using SIEM (Splunk):</H1>
<H3>by: Michael Kramer, Mike Smith, Erik Skrivseth, Darryl Anderson, Kamal Yusuf, Tubee Aden, Rennar Blaskowski, Encar Monica, Lucas Souza, Abdirahman Jama, Guled Salad</H3>


[Our Presentation](https://docs.google.com/presentation/d/170tnR21vw4NwieLy5nukslYFhyOKcX_iCS7ZB_j8Jbc/edit?usp=sharing)


Our Project covered the following:

        1. Our Monitoring Environment
        2. Attack Analysis
        3. Project Summary & Future Mitigations

<h2> The Monitoring Environment </h2>

This project used Splunk.

The Scenario:

Day 1: We are SOC analysts at Virtual Space Industries (VSI), a virtual-reality design company.We are monitoring and created SIEM Solutions for:

        https://vsi-corporation.azurewebsites.net/
        Apache web server
        Windows OS, running VSI’s back-end operations.

Day 2: Day 2: VSI has experienced sever cyber attacks and our Siem solutions from day 1 will be used to analyze the attacks.

<h2> Website Monitoring </h2>

![](/Project%203/P3-1.png)

We installed The website monitoring add-on in splunk because:
Gives us an HTTP response from websites of our choice.
Controls how often the website is checked.
Gives us the Response time of websites.
Keeps a history of when the website is up and it’s performance over time.
JobeCorp, VSI greatest adversary is prone to launching Distributed denial of service attacks in order to take down web applications. 
Our app allows us to make sure that VSI-Corporation.azurewebsites.net is up and running.
It also allows us to analyze how quickly that website is responding and other performance data.

<h2> Logs Analyzed </h2>

For this project we analyzed two sets of logs:

<h3>1. Windows Logs </h3>

These logs contain various data to users logging into VSI’s Windows server. This contains intellectual property of VSI’s next-generation virtual-reality programs.

<h3>2. Apache Logs </h3>

These logs contain data dealing with users connecting with VSI’s website. The information detailed varies from what part of the website they are accessing to what country they are requesting from to the connection status.

<h2> Window Logs </h2>

We created three reports for the windows logs in order to normalize the activity:

Signature Tables: A report that aligns signature id’s with signature types
![](/Project%203/P3-2.png)
Severity Levels: A report that shows the top 20 severity instances
![](/Project%203/P3-3.png)
Success Failure Report: A report designed to show the percentages of successful and failed activities on the server
![](/Project%203/P3-4.png)

We also created two alerts in splunk for windows.

![](/Project%203/P3-5.png)

And:

![](/Project%203/P3-6.png)

And Finally a Dashboard to Monitor the Windows Environment:

![](/Project%203/P3-7.png)

<h2> Apache Logs </h2>

We Also created three reports for the Apache logs, again, to normalize the activity:

HTTP Methods: A table of the HTTP Methods (GET, POST, HEAD and OPTIONS).

![](/Project%203/P3-8.png)

Domain Referrers: Top 10 Domain Referrers.

![](/Project%203/P3-9.png)

HTTP Response Codes: Different HTTP Response Status Codes.

![](/Project%203/P3-10.png)

Two alerts were also created for the Apache Logs.

![](/Project%203/P3-11.png)

And:

![](/Project%203/P3-12.png)

And our Apache Monitoring Dashboard:

![](/Project%203/P3-13.png)

<h2> Attack Analysis:</h2>
<h3>Windows Attack Summary</h3>

![](/Project%203/P3-14.png)

Reports:

1. There was a 14% increase in High Severity events during the attack.
2. There was no significant in change in percentage of failed activities.
3. There was a significant increase in the number of and types of signature_id and signature events.

Alerts:

4. There was a significant number of failure attempts in one hour well above threshold on March 25 at 8AM
5. There were no alerts triggered for high successful log on attempts as the the threshold was never reached.
6. There was only one instance in which the alert was trigger for high number of deleted accounts.

Dashboard:

7. There was a significant number of failure attempts in one hour well above threshold on March 25 at 8AM
8. There were no alerts triggered for high successful log on attempts as the the threshold was never reached.
9. There was only one instance in which the alert was trigger for high number of deleted accounts.

<h3>Apache Attack Summary</h3>

![](/Project%203/P3-15.png)

Reports:

1. HTTP Post requests increased from 106 in our baseline period of 84 hours to 1324 HTTP Post requests during the attack time period of 22 hours.
2. 65% of the referrer domains during the attack had a field of ‘-’. Our conclusion is that someone was masking this information during the attack.
3. HTTP Response Code 404 (NOT FOUND) increased from 213 responses during the baseline period to 679 responses during the attack. 404 codes mean the attacks were trying to access something that doesn’t exist but “Servers may also send this response instead of 403 Forbidden to hide the existence of a resource from an unauthorized client.” Which makes us believe they were trying to access unauthorized information. 
[Source](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

Alerts:

4. Our alert based on international activity didn’t increase during the attack, which tells us our threshold for the first alert was fine.
5. Our threshold for the number of HTTP POST was correct because it caught the huge increase in POST requests.

Dashboard:

6. GET requests increased first and then POST requests increased dramatically during the attack. 
7. Our Geographic statistics and IP location indicated that the amount of traffic from Ukraine increased by a large amount. This traffic specifically originated in Kiev and Kharkiv.
8. Chef Client accounted for almost 15% of the user agent traffic during the attack. Referencing opcode.com, which runs chef client (An automation platform for managing networks.) This makes me suspect that a chef client server was compromised and used in the attack.
9. VSI_Account_logon.php was the target of 30% of the URI during the attack.

<h2> Summary & Mitagation: </h2>

In conclusion, someone attempted to maliciously brute force or dictionary attack the windows login at 2a.m. on march 25th. The threat actor then tried to reset passwords to gain access to the accounts at approximately 8a.m. Two accounts in particular were targeted, User_A and User_K
We also believe that someone engaged in an HTTP flood attack beginning at 5-6pm with HTTP GET request. The attack then switched between 7-8pm to HTTP POST requests. The DDoS attack possibly used a compromised chef client server and the goal was to hinder access to the website by stopping people from using the VSI_Account_logon page.
We believe this attack originated from ukraine based on IP location data, and the fact that the attacks would have begun between 9 and 10 a.m. Ukrainian time.

<h3> Windows Mitigation: </h3>

The windows systems password policy needs to become more strict. We believe that changing the lockout policy to escalate the lockout time after multiple failed attempts to login would be the best. Also if they are locked out more than twice in a given time span VSI should require the user to contact IT before attempting to login again. We also should suggest that VSI implement a policy that user password resets must be done through IT rather than leaving the option to user as the second attack clearly showed they attempted to reset a password.

<h3> Apache Mitigation: </h3>

Apache web server has several different modules that can be used to mitigate DDoS attacks. One method is to require captcha tests for new HTTP POST requests. Otherwise VSI could install a module such as Mod_evasive Apache Module or DDOS Deflate Module. Both monitor IP addresses and ban IPs that try to open too many Connections.
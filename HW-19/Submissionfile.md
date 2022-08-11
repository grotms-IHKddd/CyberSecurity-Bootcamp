<h1>Step 1:</h1>
<h2>Background:</h2>
As the worldwide leader of importing and exporting, Vandalay Industries has been the target of many adversaries attempting to disrupt their online business. Recently, Vandaly has been experiencing DDOS attacks against their web servers. Not only were web servers taken offline by a DDOS attack, but upload and download speed were also significantly impacted after the outage. Your networking team provided results of a network speed run around the time of the latest DDOS attack.
<h2>Task:</h2>
Create a report to determine the impact that the DDOS attack had on download and upload speed. Additionally, create an additional field to calculate the ratio of the upload speed to the download speed.

    source="server_speedtest.csv" host="51e0c29bcf07" sourcetype="csv" | eval ratio = 'UPLOAD_MEGABITS' / 'DOWNLOAD_MEGABITS' | Table _time IP_ADDRESS UPLOAD_MEGABITS DOWNLOAD_MEGABITS ratio
![](/HW-19/Step_1_Table.png)

The attack began at 14:30 on February 23rd, and continued until approximately 22:30 on February 23rd, though the attack began to abate several hours earlier at 20:30 on February 23rd. By 23:30 the system seemed to have completely recovered.  

<h1>Step 2:</h1>
<h2>Background:</h2>
Due to the frequency of attacks, your manager needs to be sure that sensitive customer data on their servers is not vulnerable. Since Vandalay uses Nessus vulnerability scanners, you have pulled the last 24 hours of scans to see if there are any critical vulnerabilities.
<h2>Task:</h2>
 Create a report determining how many critical vulnerabilities exist on the customer data server. Then, build an alert to notify your team if a critical vulnerability reappears on this server.
 <h2>Report:</h2>

![](/HW-19/step_2_report.png)
<h2>Alert:</h2>

![](/HW-19/Step_2_Alert_1.png)
![](/HW-19/Step_2_Alert_2.png)

<h1>Step 3:</h1>
<h2>Background:</h2>
A Vandaly server is also experiencing brute force attacks into their administrator account. Management would like you to set up monitoring to notify the SOC team if a brute force attack occurs again.
<h2>Task</h2>
Analyze administrator logs that document a brute force attack. Then, create a baseline of the ordinary amount of administrator bad logins and determine a threshold to indicate if a brute force attack is occurring.
<h2>Logs</h2>

![](/HW-19/Step_3_Graph.png)

The attack began at approximately 8am on February 21st, and ended at 2pm on February 21st. We decided that the baseline should be between twenty and twenty five. Setting the baseline to 20-25 covers all of the events prior to the brute force attack. We decided that setting a threshold of 30 would give the normal amount some variability without risking creating false positives while still giving us some early warning of future attacks.

<h2>Alert</h2>

![](/HW-19/Step_3_Alert_1.png)
![](/HW-19/Step_3_Alert_2.png)
![](/HW-19/Step_3_Alert_3.png)

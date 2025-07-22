# Web Application Firewall (WAF) Attack Protection with Safeline

In this subdomain of the project, we’ll be configuring **Safeline WAF** to detect and block a wide range of common web application attacks. Our testbed is the a deliberately vulnerable web app (**DVWA**) behind the WAF, with attacks simulated from a **Kali Linux machine**.

This setup will demonstrate how to:
- Apply and customize WAF security rules
- Simulate attacks such as SQL Injection, XSS, HTTP flooding
- Observe and analyze WAF responses in real-time
- Harden DVWA by tuning detection policies

### HTTP Flood Defense
Http flood is a type of denial of service attack in which a threat actor sends multiple HTTP requests to overload and disrupt the availability of a server.  
To set this up:
- on the safeline dashboard go to Applications menu 
- Under the DVWA application menu select **HTTP Flood**
- Rate limiting>customize
- Enable **basic access limit**
- For this lab I'll use 
``` 
If an IP makes more 5 requests in 10 seconds block IP for 5 minutes
``` 
Now if we try and multiple incorrect password attempts in under 10s we'll be met with an error

<img width="1364" height="693" alt="image" src="https://github.com/user-attachments/assets/2c96c3f6-90ec-46e4-ba06-bbf3eb8910a8" />

We can also see details of the blocked attack on the safeline dashboard

<img width="1220" height="673" alt="image" src="https://github.com/user-attachments/assets/738421c5-2562-4ea9-832f-d99e1b1936ca" />

### Creating custom rules
Safeline also allows you to create custom allow/deny rules for specific IP addresses, request headers, or URI patterns. In this case, we’ll demonstrate how to deny access from a specific IP address:
- On the Safeline Dashboard, go the **Allow/Deny** menu.
- Navigate to Rules → Access Control → Custom Rules.
- Click Create Rule and configure the following:
- Rule Type: Deny
- Matching Type: Source IP Address
- Operator: Equals to
- IP Address: I'll use the source IP address of the http floods
- Action: Block

<img width="1268" height="635" alt="image" src="https://github.com/user-attachments/assets/648c1a5f-cb5b-42c7-9102-b2e1707e1bb7" />


Once applied, any request from that IP will be blocked. we can test this by sending traffic from the blocked IP and observing the response.

<img width="1267" height="667" alt="image" src="https://github.com/user-attachments/assets/5d62c816-f392-4569-bcde-f070d3afe6ae" />

Authentication Challenge
Another really cool feature safeline has is the Auth feature or Authentication Challenge, which entails the Authentication process being handled not by the server but by safeline instead, this adds an additional security layer by requiring users to pass a browser-based validation before accessing a protected application. This is useful for:
- Preventing bot access
- Protecting sensitive endpoints from unauthenticated users

To Set It Up:
- Navigate to your DVWA application in Safeline
- Go to Advanced Settings → Authentication.
- And finally enable it

Once enabled, any new client accessing DVWA will be met with a new login page controlled by safeline
<img width="1361" height="668" alt="Screenshot 2025-07-22 134947" src="https://github.com/user-attachments/assets/b95ee706-52b2-4734-90cc-faf693413e6d" />


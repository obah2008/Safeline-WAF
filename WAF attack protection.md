# Web Application Firewall (WAF) Attack Protection with Safeline

In this subdomain of the project, we’ll be configuring **Safeline WAF** to detect and block a wide range of common web application attacks. Our testbed is the a deliberately vulnerable web app (**DVWA**) behind the WAF, with attacks simulated from a **Kali Linux machine**.

This setup will demonstrate how to:
- Apply and customize WAF security rules
- Simulate attacks such as SQL Injection, XSS, and Command Injection
- Observe and analyze WAF responses in real-time
- Harden DVWA by tuning detection policies

HTTP Flood Defense
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

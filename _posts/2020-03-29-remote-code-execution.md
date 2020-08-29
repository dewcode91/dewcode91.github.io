---
layout: post
title:  "Remote Code Execution Vs Command Execution"
date:   2020-08-29 09:03:36 +0530
categories: Bug-Bounty
---

Hi! Bug hunters, I am back with another writeup. I will try to simplify Remote Code Execution and Command Execution. Many people think both are the same vulnerability but it’s not.
Don’t be confused! Code Evaluation, Arbitrary Code Injection, and Code Execution are synonyms of Code Injection. OS injection, Command Injection, and Arbitrary Command Execution are synonyms of Command Execution.
Code injection allows the attacker to inject his own code that is executed by the application. In Command Injection, the attacker extends the default functionality of the application, which executes system commands. Let's describe both one by one.

#### Remote Code Execution (Code Injection)
According to OWASP, Code Injection is the general term for attack types which consist of injecting code that is then interpreted/executed by the application. This type of attack exploits poor handling of untrusted data. These types of attacks are usually made possible due to a lack of proper input/output data validation. RCE stands for Remote Code Execution. That allows an attacker to inject their own code remotely on the target application. It totally depends on backend technologies. PHP, JAVA, ASP.NET, Python is the most popular backend technologies.

#### Example
If an application passes a parameter sent via a GET request to the PHP include() function with no input validation, the attacker may try to execute code other than what the developer had in mind.
The URL below passes a page name to the include() function.
<http://testsite.com/index.php?page=phpinfo();>
The phpinfo() function which is useful for gaining information about the configuration of the environment in which the web service runs. An attacker can ask the application to execute his own PHP code.

#### Remote Command Execution (Command injection)
According to OWASP, Command injection is an attack in which the goal is the execution of arbitrary commands on the host operating system via a vulnerable application. Command injection attacks are possible when an application passes unsafe user-supplied data (forms, cookies, HTTP headers, etc.) to a system shell. In this attack, the attacker-supplied operating system commands are usually executed with the privileges of the vulnerable application. Command injection attacks are possible largely due to insufficient input validation. An ability to run system commands remotely on the vulnerable application called Remote command execution.
Some useful system commands (Linux)
* id — Print user information
* ls — Directory Listing
* ps — Print current process
* w — Display current user
* df — Show information about the file system
The following request and response is an example of a successful attack:
#### Request
<http://127.0.0.1/delete.php?filename=bob.txt;id>
#### Response
uid=33(www-data) gid=33(www-data) groups=33(www-data)

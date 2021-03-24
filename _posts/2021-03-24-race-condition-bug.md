---
published: false
---

## Finding and exploiting race condition vulnerbility on facebook server
 Finding bugs on facebook was on of the biggest dream of life but it was just a dream when i read about 2014. and later 2019.
If you are not familiar about race condtion the read this blog. race condition is one of the high severity and easily exploitable bug.
brfore we jump out to the issue lets talk about my struggle during find this valid. after reading anand prakash and laxman muthiyah blog i decided to 
find bug on facebook.Struglling with slow internet and electricity issue i note down all endpoint where i try to test. after spwnding 15 days finally i found an endpoint 
at facebook developers individual verification process. i was successfully able to exploit the issue and reported to facebook team.

### Issue 1: Missing rate rimit at facebook developers individual verification process.

Individual Verification is a process that allows facebook to gather information about user so they can verify your identity as a person as opposed to a business entity or organization. User can begin the verification process in the Verification section of the `App Dashboard -- Settings -- Basic panel`, or the Individual Verification tab in your account's Developer Settings panel.

During testing the above workflow, I came across the endpoint `email` which was not blocking my ip address while submitting multiple requests on facebook server. I was successfully able to exploit this behaviour and reported this issue to facebook security team.    


#### POST Request 

```
POST /apps/async/individual_verification/send_contract/?email= HTTP/1.1
Host: developers.facebook.com
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:86.0) Gecko/20100101 Firefox/86.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://developers.facebook.com/settings/developer/indie-verification/
Content-Type: application/x-www-form-urlencoded
Content-Length: 280
Origin: https://developers.facebook.com
Connection: close
Cookie: // User cookies

```

#### Impact

An attacker can send large amount of emails from facebook server to any e-mail account (Most of the bug bounty platform not accept this type reports but they do because verification endpoint. )
   





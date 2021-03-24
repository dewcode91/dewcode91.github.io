---
published: false
---

## Finding and exploiting race condition vulnerability on facebook server
 Finding bugs on facebook was on of the biggest dream of life but it was just a dream when i read about 2014. and later 2019.
If you are not familiar about race condtion the read this blog. race condition is one of the high severity and easily exploitable bug.
brfore we jump out to the issue lets talk about my struggle during find this valid. after reading anand prakash and laxman muthiyah blog i decided to 
find bug on facebook.Struglling with slow internet and electricity issue i note down all endpoint where i try to test. after spwnding 15 days finally i found an endpoint 
at facebook developers individual verification process. i was successfully able to exploit the issue and reported to facebook team.

### Issue 1: Missing rate rimit at facebook developers individual verification.

Individual Verification is a process that allows facebook to gather information about user so they can verify your identity as a person as opposed to a business entity or organization. User can begin the verification process in the Verification section of the `App Dashboard > Settings > Basic panel`, or the Individual Verification tab in your account's Developer Settings panel.

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

During testing the above workflow, I came across above described post request where endpoint `email` can control by an attacker, which was not blocking my ip address while submitting multiple requests on facebook server. I was successfully able to exploit this behaviour and reported this issue to facebook security team.    

#### Impact

An attacker can send large amount of emails from facebook server to any e-mail account (Bug bounty platforms are not accept this type reports but facebook accepted because of verification endpoint)
   
### Issue 2: Missing rate rimit at facebook Business Verification .

Business Verification is a process that allows facebook to gather information about users and Business so we they verify your identity as a business entity. Apps that allow other Businesses to access their own data must be connected to a Business that has completed Business Verification. Until then, app users from other Businesses will be unable to grant these apps permissions and all features will be inactive.

Facebook Business is an application which allows a user to create a profile on facebook platform and provide an opportunity to promote their business in facebook platform. During my testing, i have found a missing rate limit issue on business verification where a user required to input a 5 digit code receiving via email.

```
POST /business_verification/challenge/verify/ HTTP/1.1
Host: business.facebook.com
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://business.facebook.com/settings/security/business_verification?business_id=xxxxxxxxxxxxxxx
Content-Type: application/x-www-form-urlencoded
Content-Length: 520
Connection: close
Cookie: // User cookie

submission_id=xxxxxxxxxxx&challenge_code=67890&challenge_type=email&indexed_id&__user=xxxxxxxxxxxxx&__a=1&__dyn=7xeUmFoO2CeCExUS2qq7E-8GAdyedKnFwn8eVEpyA5EK32q1oxy5Qdgdp98SmaDxW4E8U6ydwJyFEeo8p8-cx210wExuEixycx68w825ocEixWq1owvo7OqbwOzXwKzUeA9wRyUvyolyU6XximbDxeiUdo62iczErK2x0ZxzyGw8nz8a84q1UKh7wg8OqawywWg8oty88E4u2l2Utgvx-6U4a78K0AEbGg9ojwgEmy8eE&__req=y&__be=1&__pc=PHASED%3Abrands_pkg&dpr=1&__rev=1000997435&__s=%3Aen9sbg%3Axzvz6h&__hsi=6719306340508947313-0&fb_dtsg=AQFxSKvkuzNy%3AAQGIG2HsP1Ju&jazoest=22133
```


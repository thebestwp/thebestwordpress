---
title: "Security"
date: 2022-07-10T15:39:58Z
editPost:
    URL: "https://github.com/thebestwp/thebestwordpress/tree/main/content"
    Text: "edit"
    appendFilePath: true # to append file path to Edit link
summary: There is no such thing as a *secure website* on the internet.
showtoc: true
draft: false
---

In order to discuss this topic effectively it is best to use the word "secure" as a verb rather than as an adjective.

There is no such thing as a *secure website* on the internet.
Every website needs to *be secured* from plausible threats **without over-spending** on security.
The degree to which each website should be secured must be individually assessed.

So, if we're not trying to make a secure website then what are we doing?

**We are trying to achieve a reasonable level of security within our means.**

## The Golden Rule
The real key with securing a Wordpress website is to follow this one golden rule:

> Don't put sensitive information on the internet.

What you consider sensitive may be a personal choice or perhaps a legal requirement in your jurisdiction, but the rule stands--if you absolutely must prevent some information from becoming public then do not put that information on an internet-connected device.
Under no circumstances should Wordpress be used to store sensitive information.

The fact remains that people/companies/governments put indisputably sensitive information on the internet everyday, and even when there are [massive hacks and data leaks](https://www.upguard.com/blog/biggest-data-breaches) the public rarely seems [notice or care](https://www.si.umich.edu/about-umsi/news/data-breaches-most-victims-unaware-when-shown-evidence-multiple-compromised).
This happens for one very unsurprising reason: **people are stupid**.

I am pretty stupid myself.
Life is an endless series of mistakes and each with its own lesson.

## "Good Enough" Security
In practice most Wordpress sites exist to publish rather than conceal information.
More bluntly: hackers don't care much about your web site in particular. 
The kind of attackers that roam the web looking for vulnerable Wordpress sites have a lot of targets to choose from and they're only interested in the ones that are easy and inexpensive.
You must secure Wordpress against the attacks that are within your adversary's budget.

> *It doesn't take much to not be the lowest hanging fruit on the vine.*

If you leave a Wordpress site online without updates for years there is a high probability that it will become a porn site.
If, on the other hand you enable [auto updates](/posts/updates) then your site will eventually develop bugs but you will most likely evade the laziest hackers.

## Security Best Practices
[I am working on a post for each one of these topics.]
1. Hardening
    - firewall
    - no password authentication for ssh
    - read only file permissions for the web server
    - fresh salts in wp-config.php
    - no phpmyadmin
    - prevent information disclosure
    - strong passwords for login
    - use roles to restrict user permissions
1. [Backups](/posts/backups)
    - multiple locations
    - daily
    - keep backups for at least a week
    - test the backups
1. Response
    - admin on-call 7 days, ideally 24/7 
    - use floating IP's to eliminate DNS propagation delays
    - provision fresh VPS 
    - restore from backup
    - generate new random salts
    - transfer IP to new VPS
    - all within 90 minutes
    - practice this process and test backups simultaneously
1. Monitoring
    - uptimerobot
    - server resource usage alerts
    - avoid the "boy who cried wolf" problem
1. [Cloudflare](/posts/cloudflare)
    - first defense against malicious traffic
    - caching to reduce load on VPS
    - elevated security for login/search/forms
    - block requests to xmlrpc
1. [Updates](/posts/updates) installed frequently and manually reviewed.

## Cost
> Security is a bottomless money pit

Wordpress websites are easy to build because 99% of the work is done by volunteers that you will never meet and whose work you will never inspect.
If one were to independently inspect and vet every line of code then the cost benefit advantage of Wordpress would vanish.
Wordpress is therefore only as secure as the trust you place in these unknown software developers.

Key to understanding the value proposition of Wordpress is considering how important security really is to your website.

## Common Threats and Simple Solutions

### DoS
Denial of service (DoS) occurs when your server has been asked to do more work than it is capable of.
If we exclude DDoS (see below) attacks then then most common causes of DoS are: 
- inefficient code which consumes too much power per request
- legitimate traffic (aka the Slashdot effect)
- aggressive security scans

#### Solutions
- A WAF may be implemented to ban IPs that make too many requests in too short a time.
- Upgrade server infrastructure.
- Refactor code for efficiency.
- Use caching to reduce load on the server.


### DDoS Attack
A dynamic denial of service attack is usually performed by using a botnet to overwhelm the target with requests that originate from so many different IP addresses that it is difficult to block them preemptively.

#### Solutions
Cloudflare offers free DDoS protection and uses information learned from attacks on other websites to help yours.
All the DoS solutions (above) are also relevant for DDoS.


### Brute Force Login Attack
Wordpress has a nasty habit of leaking usernames and so it is common to see a consistent flow of failed login attempts from every user on the site.
These brute force attacks are almost certainly using lists of the most commonly used passwords rather than randomly generated ones.
It is practically impossible for a brute force attack on WP to successfully guess a randomly generated password with more than NN bits of entropy. 
These attacks can however cause DoS by overwhelming the server as it tries to process all the login requests.

#### Solutions
- Use strong passwords.
- A Cloudflare "page rule" may be used to elevate the security level on the WP login page to "I'm Under Attack" which will help to deflect much of the traffic before it can hit your site.
- Cloudflare's "Bot Fight Mode" will also force known botnet traffic to do a bunch of CPU intensive work to view your website, (spoiling the milk) making you a less attractive target.


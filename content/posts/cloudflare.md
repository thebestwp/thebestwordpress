---
title: "Cloudflare"
date: 2022-07-08T23:39:02Z
draft: true
editPost:
    URL: "https://github.com/thebestwp/thebestwordpress/tree/main/content"
    Text: "Suggest Changes"
    appendFilePath: true # to append file path to Edit link
---

A constant stream of unsophisticated attacks are an ever-growing nuisance to every Wordpress site.

- brute-force attacks on wp-login.php and xmlrpc.php
- aggressive security scans by random academics
- surges of script-kiddies after every new WP vulnerability disclosure

The threat from these petty attacks to a regularly updated Wordpress site is negligible but the sheer bulk of the bad traffic causes an uncomfortable load on web servers, sometimes resulting in an unintentional Denial-of-Service.

CloudFlare has emerged as the clear winner in the category of free solutions to sanitize traffic before it hits a web server. 

## Free Cloudflare Features

### Caching
The Cloudflare cache alone reduces load on the origin server significantly, freeing up resources and improving DDoS resilience.

Statistics from some real production sites over a 24h period:
- Site A: 968.39 MB saved / 2.38 GB total bandwidth (40%)
- Site B: 1.1 GB saved / 1.38 GB total bandwidth (80%)
- Site C: 3.18 GB saved / 3.36 GB total bandwidth (95%)

While bandwidth itself is not a scarce resource for small sites, this reduction in traffic means that each origin server is going to have a lot less work to do.
CloudFlare reduces traffic to the origin server.

### Web Application Firewall
The WAF provides protection from known threat IPs.
Cloudflare learns about these threats from traffic across its entire network and updates the rules for you.
You can also set up 3 of your own WAF rules for free; some examples:
- block all traffic except from a specific IP
- elevate the security level for traffic from a particular country or continent
- I can't think of a third one but you get the idea

### Bot Fight Mode
If you have ever seen the Cloudflare CAPTCHA screen and suddenly your laptop starts getting hot and loud then you may have been accidentally identified as a bot.
This happens because Cloudflare is forcing computers that it perceives as bot traffic to solve a tricky puzzle in javascript, which consumes time and CPU power.
Although this is a minor inconvenience for a real person, botnets that are targeting thousands or millions of sites will suffer real economic losses (on their power bill) as well as lengthy delays.

### Security Level
For situations where a focused DDoS attack is targeting a client site, the Security Level for the whole domain may be elevated to "I'm Under Attack" with the flip of a single switch on the dashboard.
This setting will raise the bar 

Although a CAPTCHA could be deployed on the VPS at DigitalOcean the load would still fall squarely on the VPS and an DDoS would overwhelm it.
When using  the cheaper VPS plans < $20/mo. a third party is the only option to defend against DDoS and Cloudflare is the best value in this category.

### Page Rules
Page Rules can be used to permanently elevate the Security Level to "I'm Under Attack" on specific URLs like `/wp-login.php?*`, `?s=*` and `/checkout/`

## Cloudflare Risks
By routing all of the web site's traffic through Cloudflare we expose ourselves to a variety of new threats:
- Cloudflare is now the authority on what your web site is
- Cloudflare can change your website as it is being delivered to the user
- Cloudflare can spy on your users and monetize their data
- Cloudflare can play dirty tricks to motivate you to upgrade

From a data privacy point of view there is an enormous amount of risk inherent in routing your data through a third party.
The only defence we have against these particular threats is a legal one.
The cost to win a lawsuit against Cloudflare should be considered as it is the only recourse in the event of misconduct.

Having said all that, Cloudflare has an excellent reputation so far.

### Oops
Cloudflare occasionally deploys some bad code which unintentionally knocks out huge sections of the web.

https://blog.cloudflare.com/cloudflare-outage-on-june-21-2022/  
https://blog.cloudflare.com/cloudflare-outage-on-july-17-2020/  

### Vendor Lock-In
It is important to resist the temptations that lead to vendor lock-in.
Each feature at Cloudflare should be considered as a vendor-lock threat before it is deployed in production.
Fortunately CF seems to make an effort to prevent vendor lock-in, such as their option to export DNS records for quick migration to another platform.



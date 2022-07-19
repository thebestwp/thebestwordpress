---
title: "Updates"
date: 2022-07-10T15:41:29Z
editPost:
    URL: "https://github.com/thebestwp/thebestwordpress/tree/main/content"
    Text: "edit"
    appendFilePath: true # to append file path to Edit link
summary: Installing updates automatically is better than not installing updates at all but lazy and reckless compared to every alternative approach.
draft: false
---

If you neglect to update Wordpress then your site will get hacked.
Whenever a new vulnerability is publicly disclosed the attacks start immediately.
The constant slew of updates released are patches against *known* vulnerabilities, but the ones that have yet to be publicly disclosed are still in there.

> All software has vulnerabilities.

## Automatic Updates
Installing updates automatically is better than not installing updates at all but lazy and reckless compared to every alternative approach.

The best ways to install open source software updates:
1. Read every line of code.
1. Read all related documentation/discussion about an update.
1. Manually install and test updates individually in a staging environment.
1. Same but test all updates together.
1. The previous two options but in production not staging.
1. Automatic updates with manual testing.
1. Unattended automatic updates.

Some [good advice](https://docs.fedoraproject.org/en-US/quick-docs/autoupdates/) from the Fedora project:
> "If the machine is a critical server, for which unplanned downtime of a service on the machine can not be tolerated, then you should not use automatic updates. Otherwise, you may choose to use them."

**Automatic updates will eventually cause issues with your website.**

My preferred approach to automation is *semi-automation* where I develop workflows to enhance a developer's skill without removing them from the process.

I have come to believe that achieving 80% automation with the remainder made up by human labour is orders-of-magnitude more cost effective than any attempt to achieve 100% automation, but that's a topic for another post...

## Core Updates
Wordpress core updates are absolutely essential and should be installed immediately.

### Major version upgrades
While upgrading from Wordpress version 5.9.1 to 5.9.2 is a no-brainer, going from 5.9.2 to 6.0.0 (what we call a major version release) must be considered more carefully.
Changes in user experience and/or plugin incompatibilities are common consequences of major version upgrades.
Fortunately the Wordpress developers continue to release security patches for these older versions (5.9.3 etc.) for quite awhile. 

## TODO
- Php Version
- Paid Plugins
- Manual updates

## Draft
The first thing to consider when updating Wordpress is *what* you are installing.

## Code
Every Wordpress site is a collection of different software packages, at the very least:

 | 
---|---|---
core | plugins | theme 
php | web server | db server
OS | firewall

Most Wordpress sites also have some custom code and possibly some additional frameworks like node.js or CSS pre-processors.
Most servers are also in close proximity to many other websites/servers and a ton of network equipment, each of which can themselves become hostile to your website.

So, there are a lot of things to update.

## Why we update
The reason for updating is covered by [Security](/posts/security).

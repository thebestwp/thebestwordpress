---
title: "Maintenance"
date: 2022-06-29
summary: "What is Wordpress maintenance, exactly?"
draft: false

---
Every Wordpress site is comprised primarily of thousands of lines of code written by talented hard-working volunteers whose existence we gleefully ignore.
All software has bugs and vulnerabilities which are exposed over time, more quickly when it is popular software.
We rely therefore on the benevolence of our humble open-source software developers to rapidly provide [security](/posts/security) updates to us, for free, for all eternity.

> ***Our burden is that we must install the updates.***

Maintenance is about more than just [installing updates](/posts/updates).
In order to properly maintain any software you need some skill, and that skill is generally hired.
This post is about how an agency can handle the maintenance of Wordpress sites running on VPS as opposed to the common approach of using managed, shared servers.

In order to better understand what is meant by "Wordpress maintenance" I have defined five tiers representing common situations.

Definitions and Assumptions:
- All websites are web applications and all web applications are websites
- These applications have some public facing presence on the web (maintenance is a lot less vital for things not on the internet)
- "Dependencies" includes the whole stack (excluding physical infrastructure): operating systems, software libraries, plugins, etc.


### Tier Zero
> *"Drive it into the ground"*

The client has opted out of maintenance entirely.
As time passes without updates performance degrades, bugs appear, and security vulnerabilities are targeted with increasing frequency.
The more time that an application remains in this state the more costly it will be to undo the damage.
Applications should be repaired and their maintenance tier elevated before any features are added. 

When a client stops paying for even basic maintenance:
- remove agency emails and accounts from site
- disable and delete off-site backups
- archive the git repo (if in the agency account)
- delete the site from the deployment pipeline


### Tier One
> *"Keep it running"*

This tier is about keeping the software running and secure without any changes to the code beyond those required to update dependencies.
The application will possibly degrade over time if new data is being added.
This is the bare minimum, and although attractive to budget conscious clients will probably result in higher maintenance costs over the long term.
If the site has not been well-maintained in the past there may be added up-front costs to whip it into shape.
- Operating system updates
- Off site backups
- Renew SSL certs
- Hardware upgrades as required
- Version control (git)
- Issue tracker (gitlab)
- Deployment pipeline maintenance
- Security patches (minor version updates) for all dependencies
- Major version upgrades for dependencies when EOL
- Monitoring for site uptime


### Tier Two 
> *"Keep it running well"*

Most sites being actively used will reveal (non-security) bugs over time.
Clients opting for T1 might be happy finding workarounds but in T2 bugs are fixed with quick turnaround. 
T2 should be sufficient for well-written software that doesn't need to scale rapidly.
The cost to implement T2 is high due to each site getting individualized configuration.
This is the minimum recommended tier to keep applications from degrading.
- Bug fixes
- Hardware upgrades to improve performance
- Configure cache for optimal performance
- Monitoring system resource metrics (CPU, RAM, disk, network)
- Reviewing logs after high resource consumption
- Major version upgrades for dependencies well before EOL
- Emergency response (on-call 9-5 M-F)


### Tier Three 
> *"We rely on this software"*

Some applications are mission-critical to a business and downtime equals lost revenue.
They may also contain sensitive or financial data.
T3 clients are not high profile and are unlikely to be specifically targeted in an attack.
This is a premium plan with 24h emergency tech support.
- Security scans and recommendations
- Check and adjust SEO
- Temporary hardware upgrades for special events
- Load testing and performance optimizations (in the code)
- Clean up and optimize the database
- Major version upgrades when available
- Emergency response (on-call  24/7)


### Tier Four 
> *"Our application needs to scale"*

When rapid user growth is anticipated then servers are generally compartmentalized and load balancing put in place.
T4 is also for clients who face the threat of attack by a sophisticated adversary. 
Support is provided by dedicated staff familiar with the application and familiar to the client.
- distributed systems architecture and maintenance
- reviewing critical vulnerabilities in all systems and patching ahead of official release
- strict authentication protocols for all personel
 


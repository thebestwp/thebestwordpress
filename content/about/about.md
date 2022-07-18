---
title: "About"
date: 2022-06-22
summary: It was about 24 hours before I realized that Wordpress was the wrong choice for this project...
editPost:
    URL: "https://github.com/thebestwp/thebestwordpress/tree/main/content"
    Text: "edit"
    appendFilePath: true # to append file path to Edit link
draft: false
---
> *"thebestwordpress.site is not The Best Wordpress Site"*

The Best Wordpress Site is a hypothetical ideal which no actual client will ever agree to make.

It's not this website.

This website is *about* The Best Wordpress Site.
Despite its many flaws Wordpress is (by far) the most popular platform to build a website on and one of the last holdouts against the trend of just putting everything on Facebook.
Like it or not, we're stuck with Wordpress.

Creating a Wordpress website is easy, but screwing it up is even easier.
This website is one professional's opinion on how to decide whether Wordpress is the right tool for a job, and how best to protect Wordpress in the hostile environment of cyberspace.
 
## About Wordpress…
> *"thebestwordpress.site is not a Wordpress site."*

At first I built thebestwordpress.site using Wordpress on a VPS origin server behind [Cloudflare](/posts/cloudflare) as a demonstration of what I think is the best way to serve most Wordpress sites.
It was about 24 hours before I realized that Wordpress was the wrong choice for this project because of *me.*

I am drafting a post about who Wordpress is best suited for but the shortlist is basically this:

1.  People who aren't software developers.

Wordpress has a million things that I don't need, it requires constant maintenance and is easily overwhelmed by even the most basic attack.
Even the most stripped-down basic Wordpress install is many orders-of-magnitude larger and more complex than a basic blog needs to be.

I decided that it's more difficult to do Wordpress right than it is to make an equivalent blog using any other means.
Actually building The Best Wordpress Site negates the benefit of using Wordpress.
Basically, if you're buiding a Wordpress site then you must accept that you're not trying to build the best anything.

## About thebestwordpress.site
> *"thebestwordpress.site is 176KB."*

After realizing that Wordpress was the wrong tool for the job I decided to try out a static site generator written in my favourite programming language, go.

[Hugo](https://gohugo.io) is a static site generator which means that this whole site is just a bunch of pre-rendered content.
I have written more about [how thebestwordpress.site is built.](/about/hugo)

## About me
I have been working in software for about 24 years now, and I have worn many different hats.
For the past 7 years or so I have been (among other things) the system administrator for a rotating queue of a few dozen Wordpress sites, most small, some medium sized.

This site is a necessary outlet for me to express all of my thoughts on Wordpress that no client ever wants to hear.

> *They ask, "Is Wordpress secure?"*  
> *"Nothing is secure," I cry.*


The opinions expressed on this site reflect my personal experience as the system administrator for several clients running Wordpress on Linux VPS servers.
There are a million ways to run Wordpress and this just happens to be the best one.

The workflow I will write about prioritizes client-owned accounts and open-source software for maximum portability and maintainability.
Wherever possible I strive to find the most basic and well-established open source tool for the job, which is why you will see me using Linux, ssh, git, rsync, vim--and avoiding stuff like AWS, Sublime, VSCode, docker etc.. 
I make an exception for the Cloudflare free tier as the value offered by this middleman is too great to pass up for clients without law enforcement in their threat model.

There will be no contact information listed on this site because the volume of spam would be too high.
You may however contact me through [github](https://github.com/thebestwp/thebestwordpress), if you know how.

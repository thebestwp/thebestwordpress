---
title: "Clear Linux"
date: 2022-07-15T06:44:02Z
editPost:
    URL: "https://github.com/thebestwp/thebestwordpress/tree/main/content"
    Text: "Suggest Changes"
    appendFilePath: true # to append file path to Edit link
draft: false
---
I have spent all day figuring out how to get Clear Linux working at Lunanode and the key was apparently to use the LTS kernel.
This makes sense as the native kernel is specifically targeted at "bare metal" accoring to the maintainer, Intel.

> [Clear Linux is fast.](https://www.phoronix.com/scan.php?page=article&item=h1-2022-linux&num=8)
> I mean, you might think that your Model 3 is a little zippy but that's just peanuts to Clear Linux.

The Lunanode custom image upload page instructs me that it will accept either an ISO image (which may then be run via the browser-based VNC console) or something called qcow2.
It does not apparently support any other format, nor does it claim to support compression, so it came as no surprise that the clear-kvm.img.xz failed completely when I tried it.

My next effort was to replicate [what had worked](https://docs.01.org/clearlinux/latest/get-started/cloud-install/digitalocean.html) at DigitalOcean--using the clr-installer and a yaml config to create a custom img. 
I created a 4000MB ISO, bzip2'd it down to 777MB, uploaded it to a web server and then gave the download URL to Lunanode.

It would not boot.

That was a laborious process, so I backed away to think about things.

The native kernel attempts had worked briefly but would lock up shortly after I logged in, and become unrecoverable.
I thought the problem might be my use of the virtio driver, which I had been warned about by Lunanode during the image upload process.
When I switched from virtio to IDE and booted again I suddenly had the idea to switch to the LTS kernel.

> It worked.

I struggled through the rest of the server configuration which is just different enough from Fedora to be a pain, but the site is now hosted on Clear Linux.

After a little tweaking thebestwordpress.site got a perfect score at PageSpeed insights and a 99 from pingdom with page load times under 300ms.
That's pretty much the same speed I was getting from Fedora, but it does *feel* faster.

> "Oh shit," I thought, "Was the problem the virtio driver or the native kernel?

And that brings us to now.
I need to provision yet another server.

But why?
Why fuss with a website that is getting perfect scores already?
It has been a long day already and it's going to take me at least an hour to do everything all over again.

OK, yes, but what if I can get it to load in under 100ms.

> INTERMISSION

Fortunately Lunanode made it very easy to test my theory:
1. took a snapshot of the working server 
1. modified that snapshot to use the virtio driver
1. made a new vm from the snapshot
1. moved the ip to the new vm
1. ran the pingdom test again
1. deleted the old vm

176ms will have to do until I get nginx installed.

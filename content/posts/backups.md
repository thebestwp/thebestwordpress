---
title: "Backups"
date: 2022-07-18T23:55:50Z
editPost:
    URL: "https://github.com/thebestwp/thebestwordpress/tree/main/content"
    Text: "edit"
    appendFilePath: true
draft: false
---

Although most hosting platforms offer backups these are not sufficient to protect against the hosting account being hacked or the provider going offline.
Plugins to backup from within Wordpress are a bad idea because any compromise of your web site exposes all the credentials to access the backups.

> *Frequent backups that are isolated from production environments are cheap, easy, and non-optional.*

My preferred backup strategy is first of all to use a server which is isolated from every related system and completely inaccessible to both clients and developers.
That means a different account at a different VPS host with as little connection to client sites as possible.

The backup server has root ssh access to every web server so that it can pull any file or directory from the system.
Backups are kept in a folder structure which uses domain names for organization, and contents are updated daily using rsync.
Each domain folder is then added to a restic repository where 28 daily and 3 monthly backup snapshots are retained.
Total storage required for the restic repo is currently 1.83x the size of the rsync content.

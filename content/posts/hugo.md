---

title: "Hugo"
date: 2022-07-05
summary: This website is built with Hugo, a very fast static site generator.
draft: false
disablehljs: true

---

Wordpress is a dynamic web site platform which retrieves content from a database and renders HTML in real-time.
Although most pages can be cached in a rendered state certain functions like the back-end login and search will not receive any performance boost from caching.
While the front-end performance is greatly improved by caching, back-end functions like content-editing will often be painfully slow due to limited server resources and/or an excessive number of plugins.
The dynamic nature of Wordpress also presents numerous [security risks](/posts/security) necessitating the frequent [installation of updates](/posts/updates).


This website is made of static html and CSS.

thebestwordpress.site has:
- No php
- No database
- No cache
- No memcached, no redis, no varnish
- No search field, no contact form, no e-commerce
- No back-end to login to
- No users, no password, only key-based ssh authentication for deployment
- No graphics or javascript so far and I would like to keep it that way

There is very little for the server to do and minimal "attack surface" for hackers to target.

### Hugo

[Hugo](https://gohugo.io) is a single executable file which can be downloaded and run without being installed.
There are no dependencies.

How I am using Hugo:

1. Run `hugo new posts/hugo.md`
1. Edit the new file and write some markdown
1. Run `hugo server` and open `http://localhost:1313/` in a web browser
1. Any edits made now will auto-refresh in the open browser
1. To deploy: `hugo && rsync -avz --delete public/ user@server:/var/www/html/`

Hugo compiles thebestwordpress.site into (at time of writing) 176KB of html and css files—so fast that it is possible to see changes in realtime as I edit and save them.

The built-in local development server is meant to do this—to run alongside the code editor as a realtime preview.

Feature | Wordpress | Hugo
--- | :---: | :---:
Backend | y | n
WYSIWYG | y | n
Plugins | y | n 
Themes | y | y
Fast | n | y
CDN Support | n | y

### Theme

After a few false starts I ended up going with the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) base theme and I like it quite a bit.
Having a lightweight theme with a dark/light mode toggle was my main concern.

I was able to override the default colors easily:
```shell
mkdir assets/css/extended
cp themes/PaperMod/assets/css/core/theme_vars.css assets/css/extended/z_custom.css
```

The `z_` is important because hugo compiles all the css files in alphabetical order and the custom code must be at the bottom in order to override the theme defaults.
(That took me a while to figure out.)

### Server

This site is currently hosted at [LunaNode](https://lunanode.com) on a tiny VPS with a single core and 1GB of RAM. You can contribute to the hosting bill (\$5.50/mo) using bitcoin [here.](https://btcpay737660.lndyn.com/payment-requests/b56ac8bb-e25c-4f31-99c4-b47f348f0a17)

Without any sort of WAF or DDoS protection there is a good chance that I will receive a deluge of bot traffic.
We'll see then how many bots it takes to overwhelm a purely static site.

What follows is almost everything I did to configure the server.
It's so nice to keep things this simple.

Provision a Fedora 35 server (standard Lunanode template).
The default template comes with SELinux configured.

As root:
```shell
dnf install httpd certbot mod_ssl firewalld vim lnav
firewall-cmd --add-service=https --permanent
firewall-cmd --add-service=http --permanent
firewall-cmd --reload
chown fedora:fedora /var/www/html -R
rm /etc/httpd/conf.d/ssl.conf
```

Get a cert from letsencrypt and setup the renewal job:
```shell
certbot certonly -n --force-renew --email=nope --agree-tos --authenticator=standalone --pre-hook='/bin/systemctl stop httpd; sleep 2' --post-hook='/bin/systemctl start httpd' -d thebestwordpress.site -d www.thebestwordpress.site
```

Contents of `/etc/httpd/conf.d/thebestwordpress.site.conf`:
```apache
Listen 443 https
SSLPassPhraseDialog exec:/usr/libexec/httpd-ssl-pass-dialog
SSLSessionCache         shmcb:/run/httpd/sslcache(512000)
SSLSessionCacheTimeout  300
SSLRandomSeed startup file:/dev/urandom  256
SSLRandomSeed connect builtin
SSLCryptoDevice builtin
SSLEngine on
SSLHonorCipherOrder on
SSLCipherSuite PROFILE=SYSTEM
SSLProxyCipherSuite PROFILE=SYSTEM
SSLCertificateFile /etc/letsencrypt/live/thebestwordpress.site/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/thebestwordpress.site/privkey.pem
LogLevel error
ErrorLog logs/error_log
TransferLog logs/access_log
<VirtualHost *:80>
        ServerName thebestwordpress.site,www.thebestwordpress.site
        Redirect / https://thebestwordpress.site/
</VirtualHost>
<VirtualHost *:443>
        ServerName www.thebestwordpress.site
        Redirect / https://thebestwordpress.site/
</VirtualHost>
<VirtualHost *:443>
        ServerName thebestwordpress.site
        DocumentRoot "/var/www/html"
        BrowserMatch "MSIE [2-5]" \
                nokeepalive ssl-unclean-shutdown \
                downgrade-1.0 force-response-1.0
</VirtualHost>
```


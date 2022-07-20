---

title: "Hugo"
date: 2022-06-23
summary: This website is built with Hugo, a very fast static site generator.
editPost:
    URL: "https://github.com/thebestwp/thebestwordpress/tree/main/content"
    Text: "edit"
    appendFilePath: true # to append file path to Edit link
draft: false

---

Wordpress is a dynamic web site platform which retrieves content from a database and renders HTML.
The heavy nature of a dynamic, database-driven web platform was a bad fit for me because I wanted to keep things simple and cheap, I wanted to write posts in markdown, and I wanted to minimize maintenance.
For this blog I just wanted a dark/light theme switch on a platform that would be low maintenance and which could handle a substantial amount of traffic and noise without having to upgrade its tiny server.

I have in the distant past written a lot of HTML and CSS by hand but things have changed in 20 years.
There are a million ways to make a website today but they all ultimately produce HTML and CSS that renders in your web browser.
It has become more, not less difficult to make a basic web site in my lifetime, not least of all because of the wide range of display sizes and resolutions that smartphones introduced.

> We used to make everything for a 800x600 screen and people just had to deal with it.

Fortunately someone has made a tool which does exactly what I want.

### Hugo
[Hugo](https://gohugo.io) is a static site generator which compiles html from markdown files and css from a theme.
The result is a very fast, very secure website which is easy to update, cheap to host, and which scales extremely well.

Hugo is available (for Linux, Win, Mac) as a single executable file which can be downloaded and run without being installed.
There are no dependencies.

thebestwordpress.site has:
- No php
- No database
- No cache
- No memcached, no redis, no varnish
- No Cloudflare
- No cookies
- No analytics
- No search field, no contact form, no e-commerce
- No back-end, no login forms
- No users, no password, only key-based ssh authentication for deployment
- No graphics so far and I would like to keep it that way

[There is a little javascript for the dark/light switch and the scroll to top button.]
There is very little for the server to do and minimal "attack surface" for hackers to target.
I am not saying that Hugo is better than Wordpress as they are very different things.

> Hugo is the best choice for **me** as the writer of **this** blog.

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
Backend | **Y** | n
Plugins | **Y** | n 
WYSIWYG | **Y** | **Y**
Themes | **Y** | **Y**
Markdown | n | **Y**
Fast | n | **Y**
CDN Support | n | **Y**

### Theme

After [a few false starts](https://themes.gohugo.io/) I ended up going with the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) base theme and I like it quite a bit.
Having a lightweight theme with a dark/light mode toggle was my main concern.

I was able to override the default colors easily:
```
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
**It's so nice to keep things this simple.**

Provision a Fedora 35 server (standard Lunanode template).
The default template comes with SELinux configured.

As root:
```
dnf install httpd certbot mod_ssl firewalld vim lnav
firewall-cmd --add-service=https --permanent
firewall-cmd --add-service=http --permanent
firewall-cmd --reload
chown fedora:fedora /var/www/html -R
```
There are a couple of httpd configuration files that will conflict with my configuration but will be replaced by updates if you delete them: for example `/etc/httpd/conf.d/ssl.conf`.
I am just replacing the contents with comments until I find a better solution.

Get a cert from letsencrypt and setup the renewal job:
```
certbot certonly -n --force-renew --email=no@pe.com --agree-tos --authenticator=standalone --pre-hook='/bin/systemctl stop httpd; sleep 2' --post-hook='/bin/systemctl start httpd' -d thebestwordpress.site -d www.thebestwordpress.site
```

Contents of `/etc/httpd/conf.d/thebestwordpress.site.conf`:
```
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
### Cookies are for monsters

I see no need for cookies on this site but they have become so ubiquitous that it's a little tricky to build a modern website without them.
The GDPR pop-ups are so annoying that I just don't feel that cookies are justified on thebestwordpress.site.

Both Wordpress and Cloudflare require the use of cookies and for me this was the deal-breaker that lead me to dump them for thebestwordpress.site.
I don't need analytics, I have server logs.
This site has no advertising and I have no desire to track anyone online.
It doesn't even have any javascript--the dark mode switch and animations are pure CSS.


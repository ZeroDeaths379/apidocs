# Self Hosting

Licensed under the MIT. We will not support self hosting or copying our list whatsoever, you are on your own and you MUST additionally give credit and change the branding.

This is the source code for [Fates List](https://fateslist.xyz/)

BTW please add your bots there if you wish to support us or even 

!!! danger
    Fates List is extremely difficult to the point of almost impossible (without knowledge in python) to self host. It requires Ubuntu (support for Windows and MacOS will never be happening since we do not use it). It also needs a huge amount of moving parts including PostgreSQL 14 (older versions *may* work but will never be tested), Redis and RabbitMQ. In short: **This page is only meant for people who wish to contribute to Fates List**


## Domain Setup

1. Buy a domain (You will need a domain that can be added to Cloudflare in order to use Fates List. We use namecheap for this)

2. Add the domain to Cloudflare (see [this](https://support.cloudflare.com/hc/en-us/articles/201720164-Creating-a-Cloudflare-account-and-adding-a-website)). Our whole website requires Cloudflare as the DNS in order to work.

3. Buy a Linux VPS (You need a Linux VPS or a Linux home server with a public ip with port 443 open)

4. In Cloudflare, create a record (A/CNAME) called @ that points to your VPS ip/hostname

5. In Cloudflare, go to Speed > Optimization. Enable AMP Real URL

6. In Cloudflare, go to SSL/TLS, set the mode to Full (strict), enable Authenticated Origin Pull, make an origin certificate (in Origin Server) and save the private key as /key.pem on your VPS and the certificate as /cert.pem on your VPS

7. Download [https://support.cloudflare.com/hc/en-us/article_attachments/360044928032/origin-pull-ca.pem](https://support.cloudflare.com/hc/en-us/article_attachments/360044928032/origin-pull-ca.pem) and save it on the VPS as /origin-pull-ca.pem.

## VPS Setup

1. Download the Fates List repo on the VPS using `git clone https://github.com/Fates-List/FatesList`. Make sure the location it is downloaded to is publicly accessible AKA not in a /root folder or anything like that.

2. Download snowtuft using `git clone https://github.com/Fates-List/Snowtuft`. Make sure you have xkcdpass, python3.10 and docker compose ready. Make sure gcc-c++, libffi-devel, libxslt-devel, libxml2-devel, libpq-devel packages are installed. Run `make install` to install Snowtuft

3. Enter Fates List directory, copy config_secrets_template.py to config_secrets.py and fill in the required information on there. You do not need to change site_url or mobile_site_url fields (site and mobile_site do need to be filled in without the https://). 
4. If you have a packup, copy it to /backups/latest.bak, then run `snowtuft dbs setup`. Setup venv using `snowtuft venv setup` (you may need to run this multiple times to install all development dependencies

5. Copy the nginx conf in info/nginx to /etc/nginx

6. Restart nginx

7. Run `tmux new -s rabbit`. Then run `python3 rabbitmq_worker.py`. This must be run before running Fates as this will create a queue on RabbitMQ.

8. Run `bin/run` in the repo folder

9. Follow [this](https://stevescargall.com/2020/05/13/how-to-install-prometheus-and-grafana-on-fedora-server/) to set up Prometheus and Grafana for monitoring. Set Grafanas port to 5050. Use a firewall or the digital ocean firewall to block other ports. Do not open prometheus's port in the firewall, only open Grafana's.

Fates List probihits the monetization or resale of coins or any part of Fates List for real money.

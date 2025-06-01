## Context:

I already have experience with hosting my own stuff. I currently have a Raspberry Pi 3 setup with everything I need, that ranges from hosting websites, to database backends, discord bots but also my [personal dashboard](https://github.com/glanceapp/glance).

Since my RPI 3 is older than my sister it of course lacks power to host things that take more computing power than running a "Hello World" script. Not only did I have performance issue, my RPI was also equipped with the long outdated ARMv7 chip architecture so I couldn't even run some things to begin with. 

## My wants

With my new server I want to be able to host my own image host, movie host *(via jellyfin)*, databases and in general, learn more about managing my own server and just have fun with it. 

While endlessly trying to solve bugs only I seem to have ever encoutered is pretty annoying, the moment when you finally end that suffering and figure out the solution is incredibly rewarding and makes it all worth it.

One insanely big regret I have that costed me **A LOT** of time was not documenting every command and changed config. 

With my new server I want to cleanely document everything I do and keep everything organised. For this I read through a bunch of articles about sysadmin documentation but also about IT documentation in general. *(we already have to do a lot of documenting in school so this kills two birds with one stone).* 
While reading through the articles I especially liked this quote from [parsiya.net](https://parsiya.net/blog/2020-02-06-documentation-writing-for-system-administrators-notes/).

> “Approach each day with the assumption that anything not written down will be forgotten.”

**Articles I've read:**
1. [parsiya.net](https://parsiya.net/blog/2020-02-06-documentation-writing-for-system-administrators-notes/)
2. [faddom.com](https://faddom.com/it-documentation-9-standards-and-best-practices/)

One thing that I've read a lot was to standardise the formatting. 
I really like standardisation in code as it makes everything more readable and thus drastically improves the quality of the code, especially if you're working in teams. That's why in the past months I fell into a rabbithole of reading through big tech company guides (really liked [Google's HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.html), I'd encourage you to read through it if you find the time!)

For the my servers documentation I've decided to just ride along my HTML/CSS prefered standards and took [Google's standards](https://developers.google.com/style/text-formatting) again.

## Planning

I decided I need to plan everything ahead and look for possibles issues if I want to setup my server without any major inconveniences.

Below is a list of software that I plan on using:
*Italic means that I've used the same software on my RPI before.*

**General**
| Tool               | Description                               |
|--------------------|-------------------------------------------|
| [*Cloudflared tunnel*](https://github.com/cloudflare/cloudflared) | Securely expose local services to internet |
| [*Tailscale*](https://github.com/tailscale)         | Selfhosting a VPN into my own network so I can access my servers from everywhere |
| [*PM2*](https://github.com/Unitech/pm2)                | Keep apps running and auto-restart on crash |
| [Docker](https://github.com/docker)           | Run apps in isolated containers |

Yes I didn't use docker before as I only had 1GB or ram.


**Websites**
| Tool               | Description                               |
|--------------------|-------------------------------------------|
| [*Caddy*](https://github.com/caddyserver/caddy) | Configure reverse proxies for websites |
| [Plausible](https://github.com/plausible/analytics)      | Self hostable Google analytics alternative |

Thanks to [@seakyy](https://github.com/seakyy) for introducing me to Caddy, I really couldn't bear the suffering of using NGINX anymore. 

**Security**
| Tool | Description |
|-----------------------------|------------------------------------|
| [Fail2ban](https://www.fail2ban.org/)                                 | Protects against brute force attacks by banning IPs that mess with your SSH or other services |
| [UFW (Uncomplicated Firewall)](https://help.ubuntu.com/community/UFW) | Simple firewall to block unwanted traffic                                                     |
| [RKHunter](https://rkhunter.sourceforge.net/)                         | Scans for rootkits and suspicious files to detect hacks                                       |
| [Logwatch](https://sourceforge.net/projects/logwatch/)                | Summarizes system logs daily to spot weird activity                                           |

**Monitoring**
| Tool | Description |
|---------------------------------|-------------------------------------|
| [Glances](https://nicolargo.github.io/glances/)        | Real-time system monitoring with a web dashboard                     |
| [Auditd](https://linux.die.net/man/8/auditd)           | Tracks system events and logs important stuff |


**Database stuff**
| Tool               | Description                               |
|--------------------|-------------------------------------------|
| [Supabase](https://github.com/supabase/supabase) | Selfhostable Firebase alternative (already using Supabase with [Dajia](https://dajia.lol) so this is the easier than setting everything up again)  |
| [MonogoDB](https://github.com/mongodb/mongo) | Hosting NoSQL databases (Postgres>NoSQL but we need this in school) |
| [Postgres](https://github.com/postgres/postgres) | The superior Database option, using this for new projects |

**Fun stuff**
| Tool               | Description                               |
|--------------------|-------------------------------------------|
| [Jellyfin](https://github.com/jellyfin/jellyfin) | Media server for streaming  |
| [Lychee](https://github.com/LycheeOrg/Lychee) | Image host that generates easily shareable URLs |
| [Nextcloud](https://github.com/nextcloud/server) | Self-hosted cloud storage and collaboration platform. |
| [*Glance Dashboard*](https://github.com/samba-team/samba) | Customisable dashboard website [(here's a preview of mine)](https://imgur.com/a/ffGFafK) |


**Backups**
| Tool               | Description                               |
|--------------------|-------------------------------------------|
| [rsync](https://github.com/rsyncproject/rsync) | For backing up stuff 


## Backups
Backups are incredibly important so I made sure to think of a backup concept before.

**What to Backup**
| Data Type                    | Description                                                    |
| ---------------------------- | -------------------------------------------------------------- |
| Databases                    | Postgres, MongoDB, Supabase data dumps                         |
| Media files*             | Jellyfin media library (movies, shows, music)                  |
| Nextcloud data         | User files and collaboration data                              |
| Configuration files      | Caddy, PM2, Docker Compose, firewall, fail2ban, system configs |
| Dashboard customizations | Glance HTML/CSS/JS files                                       |

**Where to Backup**

| Storage Type    | Description                                                                  |
| --------------- | ---------------------------------------------------------------------------- |
| Offsite storage | Sync backups regularly to cloud storage like Amazon S3 for disaster recovery |
| Local storage   | In the future when I have more HDD storage I'll use them as RAID             |


**Backup Frequency**

| Data Type           | Backup Frequency                           |
| ------------------- | ------------------------------------------ |
| Databases           | Daily full dumps, incremental if supported |
| Jellyfin            | Bi-weekly backup                           |
| Image host          | Daily sync or snapshot if possible         |
| Configs and scripts | After every change or at least weekly      |
| Nextcloud data | Daily sync or snapshot if possible|

*I'm very unexperienced with backing up server data so I asked ChatGPT to create the backup frequency for me.*



**Monitoring & Alerts**

I plan on monitoring everything with daily emails / a page on my Glance dashboard showing the state. 



## Server Specs
| Component | Details                  |
|-----------|--------------------------|
| CPU       | Intel i7 10700k          |
| GPU       | Nvidia RTX 3070          |
| RAM       | 32GB 3200MHz DDR4        |
| SSD       | 1TB                      |
| HDD       | 1TB                      |
| OS        | Ubuntu Server (TUI) |

I'm planning to test out my own ML models and just learn more about ML and especially image recognition. I'm also planning on integrating Ollama on websites with with my 3070. An idea that just came to my mind is summarising all my logs with Ollama and then sending a daily review of all logs to my Email / Dashboard.

## Running Cost
I asked ChatGPT to estimate the power consumption under the services that I want to run and it gave me an estimation of around 175 watt. 

These are the estimated costs for running this 24/7:
| Timeframe | Price                  |
|-----------|--------------------------|
| Monthly (Summer) | ~30 CHF          |
| Monthly (Winter) | ~40 CHF          |
| Yearly       | ~426 CHF        |

The equation ChatGPT gave me:

$$
\text{Yearly Cost (CHF)} = \frac{(378 \times 23.8) + (378 \times 32.4) + (756 \times 28.1)}{100}
$$

The result is \~426 CHF/year

## The setup process 

...

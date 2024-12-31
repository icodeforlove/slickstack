![SECURITY NOTICE](https://img.shields.io/badge/SECURITY%20NOTICE-red?style=for-the-badge&logo=warning)

This repository is a fork of the original SlickStack project (`/littlebizzy/slickstack`), which was discovered to execute remote code from personal servers—a major security vulnerability. This fork has been thoroughly cleaned to:

- Eliminate all potentially malicious code and unsafe installation procedures.
- Ensure that all remote calls are directed exclusively to this repository.
- Offer transparent, descriptive, multi-file commits for all updates.
- Retain useful functionality while placing a strong emphasis on security.
- Remove unnecessary files, reducing the repository size from over 500MB to just 2MB.

![HOW TO USE](https://img.shields.io/badge/HOW%20TO%20USE-red?style=for-the-badge&logo=warning)

**It is strongly advised to avoid using SlickStack entirely** due to its inherent security issues, poor design, and substandard maintenance practices. 

However, if you choose to proceed and security is not your top concern, please follow these recommendations:

1. Update all external calls/mirrors to point to your own server in the `slickstack/bash/ss-functions.txt` file.
2. Alternatively, remove all external calls/mirrors entirely from the `slickstack/bash/ss-functions.txt` file.

If you maintain your own fork:
- Be cautious when pulling updates or merging, as these may reintroduce malicious code.
- Note that fully addressing these issues is beyond the scope of this repository, and I do not plan to dedicate time to resolving them comprehensively.

Lastly, keep in mind that this code only functions if your fork is made public on GitHub, which is already a flawed design. A better implementation would allow for custom forks without requiring remote calls to a public repository. Instead, users could simply perform a `git pull` and merge changes locally.


---

<img src="https://repository-images.githubusercontent.com/104382627/49a17307-d8e9-49f3-a320-b3bd9c0f5e70" />

# SlickStack

SlickStack is a free LEMP stack automation script written in Bash designed to enhance and simplify WordPress provisioning, performance, and security.

## Installation

Because it's written purely in Bash (Unix shell), SlickStack has no dependencies and works on any Ubuntu LTS machine. Unlike heavier provisioning tools like EasyEngine or Ansible, there are no third party languages required such as Python or Docker, meaning a lighter and simpler approach to WordPress servers.

The below installation steps assume that you've already spun up a [KVM cloud server](https://slickstack.io/hosting) on Ubuntu LTS, with at least 1GB+ RAM, and that you are logged in via SSH as `root`:

![SECURITY NOTICE](https://img.shields.io/badge/DO%20NOT%20USE%20EXAMPLE%20ONLY-red?style=for-the-badge&logo=warning)

You should replace the url to your own repo.

```
cd /tmp/ && wget -O ss https://raw.githubusercontent.com/icodeforlove/slickstack/master/bash/ss-install.txt && bash ss
```

From this point forward, you can manage your SlickStack server by simply using the `sudo bash` command on any one of the bundled scripts located within the `/var/www/` directory, as needed. However, in most cases there shouldn't be any need for much hands-on management as the server will intelligently run various cron jobs which connect to this GitHub repo.

You can safely re-install SlickStack anytime via `sudo bash /var/www/ss-install` without causing any conflicts or data loss since the installation process is completely idempotent.

**Note:** SlickStack requires Cloudflare to be activated on your domain before SSL (HTTPS) will be recognized as fully secure by your browser, because of its self-signed OpenSSL certificate. If you wish to use Let's Encrypt instead, be sure to change your settings in `ss-config` before running the installation.

## Modules

*Last updated: Aug 08, 2024*

| Module | Version | What does SlickStack optimize? |
| :------------- | :----------: | :----------: |
| **Ubuntu LTS** | 24.04 | `crontab` + `gai.conf` + `sshd_config` + `sudoers` + `sysctl.conf` |
| **Nginx** | 1.18.x | `nginx.conf` + `cloudflare.conf` + server blocks |
| **OpenSSL** | 3.0.x | `slickstack.crt` + `slickstack.key` + `dhparam.pem` |
| **Certbot** | 2.9.x | `cert.perm` + `chain.pem` + `fullchain.pem` + `privkey.pem` |
| **MySQL** | 8.0.x | `my.cnf` |
| **PHP-FPM** | 8.3.x | `php.ini` + `php-fpm.conf` + `www.conf` |
| **Memcached** | 1.6.x | `memcached.conf` + `object-cache.php` |
| **WordPress** | 6.6.x | some WP Core junk files removed by `ss-clean-files` |
| **WP-CLI** | 2.11.x | some `wp` commands disabled |
| **Adminer** | 4.8.1 | default config |
| **UFW Firewall** | 0.36.x | `ufw` + `ufw.conf` + `user-rules` |
| **Fail2ban** | 0.11.x | `jail.local` + custom filters |

## Requirements

*NOTE: SlickStack will never support installing multiple TLDs (multi-tenancy) on a single server. This is to ensure top speed, stability, and security (i.e. technical SEO). We will also never include any type of UI, to allow third party applications to integrate SlickStack with management tools as they best see fit.*

SlickStack works best on [KVM cloud servers](https://slickstack.io/hosting) with at least 2GB+ RAM such as DigitalOcean, Vultr, and Linode. The underlying LEMP stack configuration is meant primarily for high-traffic single-site WordPress websites, however WordPress Multisite is also supported. SlickStack supports WordPress, WooCommerce, bbPress, and BuddyPress "out of the box" with optimized settings that scale; what this means is that you can upgrade your cloud server to a bigger or better instance, and then run `ss install` again, and most SlickStack settings will be automagically optimized per available resources.

By default, MySQL will connect locally via TCP to `127.0.0.1:3306` databases called `production`, `staging`, and `development` (depending on whether you have enabled staging/dev sites or not), although remote databases also work very well. Server "clustering" or "load balancing" has not been tested, and is not the goal here; complex enterprise-style configurations for WordPress are rarely needed (and can be expensive and difficult to manage), thus SlickStack aims to provide a simple solution for the 99% of WordPress sites that don't need such complexity.

It should also be noted that SlickStack [ss] is HTTPS-only, and that HSTS is enabled by default, meaning that HTTP sites are not supported. Because OpenSSL generates self-signed certificates, SlickStack [ss] servers require CloudFlare to be active in front of your server in order for SSL certificates to be properly CA-signed and loaded by your browser, at least until the first `ss-install` has been completed (after that, you can switch to Certbot / Let's Encrypt).

## Philosophy

Outside of the so-called [Application Layer](https://en.wikipedia.org/wiki/Application_layer), so much of the way computers and servers now work has been moved away from in-house teams and specialists and onto "the cloud" that terms like [DevOps](https://www.reddit.com/r/devops/comments/3rpzem/devops_vs_sysadmin/cwqmlnd/) have become standard among recruiters, companies, and developers alike. Modern web development trends have begun to revolve entirely around concepts such as automation, APIs, [cloud services](https://www.bcsg.com/wp-content/uploads/2015/03/The-small-business-revolution-trends-in-SMB-cloud-adoption.pdf), and beyond — a phenomenon we might refer to as *Web 3.0*.

While this shift is exciting, there is now a massive and growing disconnect between these emerging technologies and the humans that are expected to implement or benefit from them. Typical small business owners (SMBs), along with independent agencies or freelancers, now face a virtually impossible learning curve if they wish to maintain a competitive "webdev" edge, let alone keep up with basic standards in website performance and security.

While Silicon Valley "gurus" and corporations pump out new SaaS services (or incredibly complex Configuration Management tools like Ansible) on a daily basis, the typical small business website is still trying to figure out how to make their contact forms work correctly. The "legacy" shared web hosting monopolies — think EIG or GoDaddy — also have little motivation to educate their audience, as perpetuating confusion seems to be a core pillar of their business model.

Thus, before the likes of Google and Amazon and Shopify and Wix take over the entire web and turn it into Wall Street-backed website builders that feed into their private ecosystems, SlickStack hopes to bridge the knowledge gap between emerging technology and old-school web development to empower SMBs to achieve top notch website performance and security by offering a "controlled" LEMP-stack environment with limited options that is perfectly suited to the world's most popular open-source CMS: WordPress.

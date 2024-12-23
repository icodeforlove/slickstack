<span style="color: red">⚠️ **SECURITY NOTICE**

This is a fork of the original SlickStack project (https://github.com/littlebizzy/slickstack) which was found to be executing remote code from personal servers - a significant security risk. This version has been cleaned up to:

- Remove all potentially malicious code and unsafe installation processes 
- Ensure all remote calls are made directly to this repository
- Provide transparent, descriptive multi-file commits for all updates
- Maintain the useful functionality while prioritizing security
- Remove unnecessary files, reducing repo size from over 500MB to just 2MB</span>

---

<img src="https://repository-images.githubusercontent.com/104382627/49a17307-d8e9-49f3-a320-b3bd9c0f5e70" />

# SlickStack

SlickStack is a free LEMP stack automation script written in Bash designed to enhance and simplify WordPress provisioning, performance, and security.

## Installation

Because it's written purely in Bash (Unix shell), SlickStack has no dependencies and works on any Ubuntu LTS machine. Unlike heavier provisioning tools like EasyEngine or Ansible, there are no third party languages required such as Python or Docker, meaning a lighter and simpler approach to WordPress servers.

The below installation steps assume that you've already spun up a [KVM cloud server](https://slickstack.io/hosting) on Ubuntu LTS, with at least 1GB+ RAM, and that you are logged in via SSH as `root`:

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

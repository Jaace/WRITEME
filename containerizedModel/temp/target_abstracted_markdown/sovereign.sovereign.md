@abstr_hyperlink @abstr_hyperlink 

# Introduction

Sovereign is a set of @abstr_hyperlink playbooks that you can use to build and maintain your own @abstr_hyperlink based entirely on open source software, so you’re in control.

If you’ve never used Ansible before, you might find these playbooks useful to learn from, since they show off a fair bit of what the tool can do.

The original author's @abstr_hyperlink might be of interest. tl;dr: frustrations with Google Apps and concerns about privacy and long-term support.

Sovereign offers useful cloud services while being reasonably secure and low-maintenance. Use it to set up your server, SSH in every couple weeks, but mostly forget about it.

## Services Provided

What do you get if you point Sovereign at a server? All kinds of good stuff!

  * @abstr_hyperlink over SSL via @abstr_hyperlink , complete with full text search provided by @abstr_hyperlink .
  * @abstr_hyperlink over SSL, also via Dovecot
  * @abstr_hyperlink over SSL via Postfix, including a nice set of @abstr_hyperlink to discard spam before it ever hits your filters.
  * Virtual domains for your email, backed by @abstr_hyperlink .
  * Spam fighting via @abstr_hyperlink .
  * Mail server verification using @abstr_hyperlink and @abstr_hyperlink so the Internet knows your mailserver is legit.
  * Secure on-disk storage for email and more via @abstr_hyperlink .
  * Webmail via @abstr_hyperlink .
  * Mobile push notifications via @abstr_hyperlink .
  * Email client @abstr_hyperlink .
  * Jabber/ @abstr_hyperlink instant messaging via @abstr_hyperlink .
  * An RSS Reader via @abstr_hyperlink .
  * @abstr_hyperlink and @abstr_hyperlink to keep your calendars and contacts in sync, via @abstr_hyperlink .
  * Your own private storage cloud via @abstr_hyperlink .
  * Your own VPN server via @abstr_hyperlink .
  * An IRC bouncer via @abstr_hyperlink .
  * @abstr_hyperlink to keep everything running smoothly (and alert you when it’s not).
  * @abstr_hyperlink to collect system statistics.
  * Web hosting (ex: for your blog) via @abstr_hyperlink .
  * Firewall management via @abstr_hyperlink .
  * Intrusion prevention via @abstr_hyperlink and rootkit detection via @abstr_hyperlink .
  * SSH configuration preventing root login and insecure password authentication
  * @abstr_hyperlink two-factor authentication compatible with @abstr_hyperlink and various hardware tokens
  * Nightly backups to @abstr_hyperlink .
  * Git hosting via @abstr_hyperlink and @abstr_hyperlink .
  * Read-it-later via @abstr_hyperlink 
  * A bunch of nice-to-have tools like @abstr_hyperlink and @abstr_hyperlink that make life with a server a little easier.



Don’t want one or more of the above services? Comment out the relevant role in `site.yml`. Or get more granular and comment out the associated `include:` directive in one of the playbooks.

# Usage

## What You’ll Need

@abstr_number . A VPS (or bare-metal server if you wanna ball hard). My VPS is hosted at @abstr_hyperlink . You’ll probably want at least @abstr_number MB of RAM between Apache, Solr, and PostgreSQL. Mine has @abstr_number . @abstr_number . @abstr_hyperlink or an equivalent Linux distribution. (You can use whatever distro you want, but deviating from Debian will require more tweaks to the playbooks. See Ansible’s different @abstr_hyperlink modules.) @abstr_number . A @abstr_hyperlink account with some credit in it. You could comment this out if you want to use a different backup service. Consider paying your hosting provider for backups or using an additional backup service for redundancy.

You do not need to acquire an SSL certificate. The SSL certificates you need will be obtained from @abstr_hyperlink automatically when you deploy your server.

## Installation

## On the remote server

The following steps are done on the remote server by `ssh`ing into it and running these commands.

### @abstr_number . Install required packages e.g `aptitude` is required on Debian
    
    
    apt-get install sudo python
    

### @abstr_number . Get a Tarsnap machine key

If you haven’t already, @abstr_hyperlink , or use `brew install tarsnap` if you use @abstr_hyperlink .

Create a new machine key for your server:
    
    
    tarsnap-keygen --keyfile roles/tarsnap/files/decrypted_tarsnap.key --user me@example.com --machine example.com
    

### @abstr_number . Prep the server

For goodness sake, change the root password:
    
    
    passwd
    

Create a user account for Ansible to do its thing through:
    
    
    useradd deploy
    passwd deploy
    mkdir /home/deploy
    

Authorize your ssh key if you want passwordless ssh login (optional):
    
    
    mkdir /home/deploy/.ssh
    chmod  @abstr_number  /home/deploy/.ssh
    nano /home/deploy/.ssh/authorized_keys
    chmod  @abstr_number  /home/deploy/.ssh/authorized_keys
    chown deploy:deploy /home/deploy -R
    echo 'deploy ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/deploy
    

Your new account will be automatically set up for passwordless `sudo`. Or you can just add your `deploy` user to the sudo group.
    
    
    adduser deploy sudo
    

## On your local machine

Ansible (the tool setting up your server) runs locally on your computer and sends commands to the remote server. Download this repository somewhere on your machine, either through `Clone or Download > Download ZIP` above, `wget`, or `git` as below
    
    
    git clone https://github.com/sovereign/sovereign.git
    

### @abstr_number . Configure your installation

Modify the settings in the `group_vars/sovereign` folder to your liking. If you want to see how they’re used in context, just search for the corresponding string. All of the variables in `group_vars/sovereign` must be set for sovereign to function.

For Git hosting, copy your public key into place:
    
    
    cp ~/.ssh/id_rsa.pub roles/git/files/gitolite.pub
    

Finally, replace the `host.example.net` in the file `hosts`. If your SSH daemon listens on a non-standard port, add a colon and the port number after the IP address. In that case you also need to add your custom port to the task `Set firewall rules for web traffic and SSH` in the file `roles/common/tasks/ufw.yml`.

### @abstr_number . Set up DNS

If you’ve just bought a new domain name, point it at @abstr_hyperlink or similar. Most VPS services (and even some domain registrars) offer a managed DNS service that you can use for this at no charge. If you’re using an existing domain that’s already managed elsewhere, you can probably just modify a few records.

Create `A` or `CNAME` records which point to your server's IP address:

  * `example.com`
  * `mail.example.com`
  * `www.example.com` (for Web hosting)
  * `autoconfig.example.com` (for email client automatic configuration)
  * `read.example.com` (for Wallabag)
  * `news.example.com` (for Selfoss)
  * `cloud.example.com` (for ownCloud)
  * `git.example.com` (for cgit)



### @abstr_number . Run the Ansible Playbooks

First, make sure you’ve @abstr_hyperlink .

To run the whole dang thing:
    
    
    ansible-playbook -i ./hosts --ask-sudo-pass site.yml
    

If you chose to make a passwordless sudo deploy user, you can omit the `--ask-sudo-pass` argument.

To run just one or more piece, use tags. I try to tag all my includes for easy isolated development. For example, to focus in on your firewall setup:
    
    
    ansible-playbook -i ./hosts --tags=ufw site.yml
    

You might find that it fails at one point or another. This is probably because something needs to be done manually, usually because there’s no good way of automating it. Fortunately, all the tasks are clearly named so you should be able to find out where it stopped. I’ve tried to add comments where manual intervention is necessary.

The `dependencies` tag just installs dependencies, performing no other operations. The tasks associated with the `dependencies` tag do not rely on the user-provided settings that live in `group_vars/sovereign`. Running the playbook with the `dependencies` tag is particularly convenient for working with Docker images.

### @abstr_number . Finish DNS set-up

Create an `MX` record for `example.com` which assigns `mail.example.com` as the domain’s mail server.

To ensure your emails pass DKIM checks you need to add a `txt` record. The name field will be `default._domainkey.EXAMPLE.COM.` The value field contains the public key used by DKIM. The exact value needed can be found in the file `/var/lib/rspamd/dkim/EXAMPLE.COM.default.txt`. It will look something like this:
    
    
    v=DKIM @abstr_number ; k=rsa; p=MIGfMA @abstr_number GCSqGSIb @abstr_number DQEBAQUAA @abstr_number GNADCBiQKBgQDKKAQfMwKVx+oJripQI+Ag @abstr_number uTwYnsXKjgBGtl @abstr_number Tk @abstr_number UMTUwhMqnitqbR/ZQEZjcNolTkNDtyKZY @abstr_number Z @abstr_number LqvM @abstr_number KsrITpiMbkV @abstr_number eX @abstr_number GKczT @abstr_number Lws @abstr_number KXn+ @abstr_number BHCKULGdireTAUr @abstr_number Id @abstr_number mtjLrbi/E @abstr_number Pq @abstr_number Zs @abstr_number hkDxsDcve @abstr_number WccjafJVwIDAQAB
    

For DMARC you'll also need to add a `txt` record. The name field should be `_dmarc.EXAMPLE.COM` and the value should be `v=DMARC @abstr_number ; p=none`. More info on DMARC can be found @abstr_hyperlink .

Set up SPF and reverse DNS @abstr_hyperlink . Make sure to validate that it’s all working, for example, by sending an email to @abstr_hyperlink and reviewing the report that will be emailed back to you.

### @abstr_number . Miscellaneous Configuration

Sign in to the ZNC web interface and set things up to your liking. It isn’t exposed through the firewall, so you must first set up an SSH tunnel:
    
    
    ssh deploy@example.com -L  @abstr_number :localhost: @abstr_number
    

Then proceed to http://localhost: @abstr_number in your web browser.

Similarly, to access the server monitoring page, use another SSH tunnel:
    
    
    ssh deploy@example.com -L  @abstr_number :localhost: @abstr_number
    

Again proceeding to http://localhost: @abstr_number in your web browser.

Finally, sign into ownCloud with a new administrator account to set it up. You should select PostgreSQL as the configuration backend. Use `owncloud` as the database user and the database name. For the database password ansible has created a set of random passwords for each service and stores them in your local folder `secret`, use the one in the file `owncloud_db_password`.

## How To Use Your New Personal Cloud

We’re collecting known-good client setups @abstr_hyperlink .

## Troubleshooting

If you run into an errors, please check the @abstr_hyperlink . If the problem you encountered, is not listed, please go ahead and @abstr_hyperlink . If you already have a bugfix and/or workaround, just put them in the issue and the wiki page.

### Reboots

You will need to manually enter the password for any encrypted volumes on reboot. This is not Sovereign-specific, but rather a function of how EncFS works. This will necessitate SSHing into your machine after reboot, or accessing it via a console interface if one is available to you. Once you're in, run this:
    
    
    encfs /encrypted /decrypted --public
    

It is possible that some daemons may need to be restarted after you enter your password for the encrypted volume(s). Some services may stall out while looking for resources that will only be available once the `/decrypted` volume is available and visible to daemon user accounts.

# IRC

Ask questions and provide feedback in `#sovereign` on @abstr_hyperlink .

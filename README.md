Description
=========

[![Build Status](https://travis-ci.org/Yannik/ansible-role-relaymail.svg?branch=master)](https://travis-ci.org/Yannik/ansible-role-relaymail)

This role setups up a host so that it sends outgoing mails over a smarthost and optionally forwards email addressed to local system users. A secure alternative to ssmtp.

Why shouldn't I use ssmtp, isn't it easier to setup?
=========

I actually believe that this role makes it even easier to setup postfix than ssmtp.

This is what I found out when I installed ssmtp myself:

>I wanted to use ssmtp today too, but noticed that it does NOT verify the SSL/TLS certificate of the remote server on the current debian & ubuntu releases and also does NOT verify the hostname of the certificate. This is a major issue, as this effectively renders the encryption useless and your password is being transmitted alike to being plaintext and anyone can sniff it. This has also been reported in a debian bug, but there has not been any progress for years: [https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=662960](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=662960)

>The ssmtp version in the Redhat packages has been patched to atleast verify the certificate, but the hostname is still NOT being verified and the encryption is therefore as insecure as on debian/ubuntu. There is a bug for this, but there is also no progress for years: [https://bugzilla.redhat.com/show_bug.cgi?id=864894](https://bugzilla.redhat.com/show_bug.cgi?id=864894)

>So, if you care about the security of the email account you use for your servers outgoing emails, do NOT use ssmtp.

>ssmtp has had no active development since atleast 2009: [https://anonscm.debian.org/gitweb/?p=ssmtp/ssmtp.git](https://anonscm.debian.org/gitweb/?p=ssmtp/ssmtp.git)

In addition to these points, any user that can send mails over ssmtp needs read-access to the ssmtp config file which includes the username and password used for smtp auth.
In normal conditions, you would probably give read permission to 'other', which would mean that for every single user/service on that system could read your smtp credentials.

This is not the case with the security-focused design of postfix.

Requirements
------------

This role works on all debian-based distributions and could easily be patched to work on any distribution which provides postfix.

Ansible version 2.0 or greater is required for this role.

Role Variables
--------------

* `relaymail_smtp_host`: hostname of the smtp server used for relaying email (required)
    * Example: `smtp.example.org`
* `relaymail_smtp_port`: port of the smtp server used for relaying email
    * Default: `25`
* `relaymail_smtp_user`: username to authenticate with at the relaying mailserver (optional)
    * Example: `user@example.org`
* `relaymail_smtp_password`: password to authenticate with at the rayling mailserver (required if user is provided)
* `relaymail_force_from_address`: force the from address to be the `relaymail_smtp_user` (you must specify a relaymail_smtp_user if true)
    * Default: `true`
* `relaymail_overwrite_to`: `all` overwrites the to address for all emails, `local` overwrites the to address for emails addressed to local users, `none` does never overwrite the to address
    * Default: `all`
* `relaymail_overwrite_to_target`: email address which mails with overwritten to should be sent to (required when `relaymail_overwrite_to` is not `none`)
    * Example: `user2@example.org`
* `relaymail_additional_options`: dictionary of key/value pairs to append to main.cf.
    * Default: {}

**Note:**  Options set using `relaymail_additional_options` will override previous settings.
Per the postfix manual, _"When the same parameter is defined multiple times, only the last instance is remembered."_
So while overrides are valid, postfix will generate a warning message

Example Playbook
----------------

    - hosts: all
      roles:
        - role: Yannik.relaymail
          relaymail_smtp_host: smtp.example.org
          relaymail_smtp_user: user@example.org
          relaymail_smtp_password: secret
          relaymail_overwrite_to: local
          relaymail_overwrite_to_target: user2@example.org
          relaymail_additional_options:
            smtp_tls_wrappermode: "yes"

License
-------

GPLv2

Author Information
------------------

Yannik Sembritzki

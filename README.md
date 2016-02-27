Description
=========

[![Build Status](https://travis-ci.org/Yannik/ansible-role-relaymail.svg?branch=master)](https://travis-ci.org/Yannik/ansible-role-relaymail)

This role setups up a host so that it sends outgoing mails over a smarthost.

Requirements
------------

This role works on all debian-based distributions and could easily be patched to work on any distribution which provides postfix.

Ansible version 2.0 or greater is required for this role.

Role Variables
--------------

* `relaymail_smtp_host`: hostname of the smtp server used for relaying email
    * Example: `smtp.example.org`
* `relaymail_smtp_user`: username to authenticate with at the relaying mailserver
    * Example: `user@example.org`
* `relaymail_smtp_password`: password to authenticate with at the rayling mailserver
* `relaymail_force_from_address`: force the from address to be the `relaymail_smtp_user`
    * Default: `true`
* `relaymail_overwrite_to`: `all` overwrites the to address for all emails, `local` overwrites the to address for emails addressed to local users, `none` does never overwrite the to address
    * Default: `all`
* `relaymail_overwrite_to_target`: email address which mails with overwritten to should be sent to (required when `relaymail_overwrite_to` is not `none`)
    * Example: `user2@example.org`


Example Playbook
----------------

    - hosts: all
      roles:
        - role: relaymail
          relaymail_smtp_host: smtp.example.org
          relaymail_smtp_user: user@example.org
          relaymail_smtp_password: secret
          relaymail_overwrite_to: local
          relaymail_overwrite_to_target: user2@example.org


License
-------

GPLv2

Author Information
------------------

Yannik Sembritzki

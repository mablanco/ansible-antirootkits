# mablanco.antirootkits

Ansible role to deploy several antorootkit tools, from the official Linux packages.

Only Debian and Ubuntu are supported, right now.

## Role Variables

- **antirootkits_mail_from**: Sender email address for the audit reports. No valid default, you have to fill it in.
- **antirootkits_mail_to**: Receiver email address for the audit reports. No valid default, you have to fill it in.

## Example Playbook

Example of how to use this role:

    - hosts: debian_servers
      vars:
         antirootkits_mail_from: 'sender@example.com'
         antirootkits_mail_rcpt: 'receiver@example.com'
      roles:
         - { role: mablanco.antirootkits }

## ToDo
- Send email reports from chkrootkit and unhide
- Schedule unhide runs

## License

GPLv3

# mablanco.antirootkits

Ansible role to deploy several rootkit and malware detection tools:

- rkhunter
- chkrootkit
- unhide
- Shell Detector (<https://github.com/emposha/Shell-Detector>)

Only Debian and Ubuntu are supported, right now.

## Role Variables

- **antirootkits_mail_from**: Sender email address for the audit reports. No valid default, you have to fill it in.
- **antirootkits_mail_to**: Receiver email address for the audit reports. No valid default, you have to fill it in.
- **antirootkits_log_expire**: Days before logs are purged. Defaults to _'90'_.
- **shelldetector_scan_directory**: Directory to scan for Shell Detector. Defaults to _'/var/www'_.
- **shelldetector_cron_hour**: Hour of execution of Shell Detector's cron job. Defaults to _'6'_.
- **shelldetector_cron_minute**: Minute of execution of Shell Detector's cron job. Defaults to _'30'_.

## Example Playbook

Example of how to use this role:

```
- hosts: debian_servers
  vars:
     antirootkits_mail_from: 'sender@example.com'
     antirootkits_mail_to: 'receiver@example.com'
  roles:
     - { role: mablanco.antirootkits }
```

## ToDo

- Send email reports from chkrootkit and unhide
- Schedule unhide runs

## License

GPLv3

# mablanco.antirootkits

Ansible role to deploy several rootkit and malware detection tools:

- [Rkhunter](<http://rkhunter.sourceforge.net/>): rootkit, backdoor, sniffer and exploit scanner
- [chkrootkit](<http://www.chkrootkit.org/>): rootkit detector
- [Unhide](<https://github.com/YJesus/Unhide>): forensic tool to find hidden processes and TCP/UDP ports by rootkits
- [Shell Detector](<https://github.com/emposha/Shell-Detector>): application that helps you find and identify php/cgi(perl)/asp/aspx shells

Debian, RHEL and their respective derivatives are supported. **chkrootkit** is not available for RHEL.

## Role Variables

### Tools to install

The following variables control whether a tool is installed (*true*) or not (*false*). All variables default to _'false'_.

- **rkhunter**
- **chkrootkit**
- **unhide**
- **shelldetector**

### General setup

- **antirootkits_mail_cmd**: Command to send reports (varies between Debian and RHEL)
- **antirootkits_mail_from**: Sender email address for the audit reports. No valid default, you have to fill it in.
- **antirootkits_mail_to**: Receiver email address for the audit reports. No valid default, you have to fill it in.
- **antirootkits_log_expire**: Days before logs are purged. Defaults to _'90'_.
- **antirootkits_rkhunter_diag_scan**: Include application check for detailed report scan. Defaults to _'no'_ (RHEL only)

### Unhide setup

- **unhide_cron_hour**: Hour of execution of Unhide's cron job. Defaults to _'6'_.
- **unhide_cron_minute**: Minute of execution of Unhide's cron job. Defaults to _'00'_.

### Shell Detector setup

- **shelldetector_install_directory**: Install directory. Defaults to _'/opt/Shell-Detector'_.
- **shelldetector_scan_directory**: Directory to scan. Defaults to _'/var/www'_.
- **shelldetector_cron_hour**: Hour of execution of Shell Detector's cron job. Defaults to _'6'_.
- **shelldetector_cron_minute**: Minute of execution of Shell Detector's cron job. Defaults to _'30'_.

### Rkhunter setup

- **rkhunter_allow_ssh_root_user**: Define what rkhunter should expect in sshd config. Defaults to _'no'_.

## Example playbook

Example of how to use this role:

```
- hosts: servers
  vars:
     antirootkits_mail_from: 'sender@example.com'
     antirootkits_mail_to: 'receiver@example.com'
  roles:
     - { role: mablanco.antirootkits }
```

## License

GPLv3

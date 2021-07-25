# mablanco.antirootkits

Ansible role to deploy several rootkit and malware detection tools:

- rkhunter
- chkrootkit
- unhide
- Shell Detector (<https://github.com/emposha/Shell-Detector>)

Only Debian and Ubuntu are fully supported, right now. On RHEL, the following antirootkits can be installed:

- rkhunter
- shelldetector

## Role Variables

The role can be used to deploy only a few of all the supported tools:

- **antirootkits_rkhunter_enabled**: wether to install and configure rkhunter
- **antirootkits_chkrootkit_enabled**: wether to install and configure chkrootkit (defaults to false on RHEL)
- **antirootkits_unhide_enabled**: wether to deploy unhide (defaults to false on RHEL)
- **antirootkits_shelldetector_enabled**: wether to deploy shelldetector
- **antirootkits_pkg_list**: list of packages to install (varies between Debian and RHEL)

- **antirootkits_mail_cmd**: Command to send reports (varies between Debian and RHEL)
- **antirootkits_mail_from**: Sender email address for the audit reports. No valid default, you have to fill it in.
- **antirootkits_mail_to**: Receiver email address for the audit reports. No valid default, you have to fill it in.
- **antirootkits_log_expire**: Days before logs are purged. Defaults to _'90'_.
- **antirootkits_rkhunter_diag_scan**: Include application check for detailed report scan. Defaults to _'no'_ (RHEL only)
- **shelldetector_scan_directory**: Directory to scan for Shell Detector. Defaults to _'/var/www'_.
- **shelldetector_cron_hour**: Hour of execution of Shell Detector's cron job. Defaults to _'6'_.
- **shelldetector_cron_minute**: Minute of execution of Shell Detector's cron job. Defaults to _'30'_.

## Example Playbook

Example of how to use this role:

```
- hosts: servers
  vars:
     antirootkits_mail_from: 'sender@example.com'
     antirootkits_mail_to: 'receiver@example.com'
  roles:
     - { role: mablanco.antirootkits }
```

## ToDo

- Schedule unhide runs and send email reports
- Add more tools!

## License

GPLv3

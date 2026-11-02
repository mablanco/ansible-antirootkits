# mablanco.antirootkits

[![CI](https://github.com/mablanco/ansible-antirootkits/actions/workflows/ci.yml/badge.svg)](https://github.com/mablanco/ansible-antirootkits/actions/workflows/ci.yml)

Ansible role to deploy rootkit and malware detection tools on Linux, scheduled to run daily and mail their reports:

- [Rkhunter](https://rkhunter.sourceforge.net/): rootkit, backdoor, sniffer and exploit scanner.
- [chkrootkit](https://www.chkrootkit.org/): rootkit detector.
- [Unhide](https://github.com/YJesus/Unhide): forensic tool to find processes and ports hidden by rootkits.

## Supported platforms

| Tool | Debian 12 and 13 | Ubuntu 24.04 and 26.04 | EL 9 and 10 | openSUSE Leap 16.0 |
|---|---|---|---|---|
| Rkhunter | ✅ | ✅ | ✅ (from EPEL) | ✅ |
| chkrootkit | ✅ | ✅ | ❌ not packaged | ❌ not packaged |
| Unhide | ✅ | ✅ | ❌ not packaged | ❌ not packaged |

Tools that are not packaged for the target system are skipped. Building them from source would need a compiler on the hosts being audited, which is a bad practice for a security tool.

- **EL:** the role installs `epel-release` on Rocky Linux, AlmaLinux and other rebuilds. On RHEL, enable EPEL before running it.
- **openSUSE:** the role needs the `community.general` collection, which has the `zypper` module (`ansible-galaxy collection install community.general`). The package does not schedule any run, so the role installs a daily cron job that reads `/etc/sysconfig/rkhunter`, plus `cron` and `mailx`. SUSE Linux Enterprise is not tested.

The hosts need a working `mail` command and mail transport agent for the reports to be delivered.

## Role variables

### Tools to install

All of them default to `false`.

- **rkhunter**
- **chkrootkit**
- **unhide**

### General setup

- **antirootkits_mail_from**: sender address of the reports. Required.
- **antirootkits_mail_to**: receiver address of the reports. Required.
- **antirootkits_log_expire**: days before the Unhide reports are deleted. Defaults to `90`.

### Rkhunter setup

- **rkhunter_allow_ssh_root_user**: value of `ALLOW_SSH_ROOT_USER`, what rkhunter should expect in the SSH server configuration. Defaults to `no`.
- **rkhunter_cron_db_update**: whether the scheduled runs update rkhunter's data files (Debian, Ubuntu and openSUSE). Defaults to `true`.
- **antirootkits_rkhunter_diag_scan**: whether the daily report includes the application check (EL). Defaults to `no`.

### Unhide setup

- **unhide_log_directory**: directory for the reports. Defaults to `/var/log/unhide`.
- **unhide_cron_hour**: hour of the daily audit. Defaults to `6`.
- **unhide_cron_minute**: minute of the daily audit. Defaults to `0`.

## Example playbook

```yaml
- hosts: servers
  vars:
    antirootkits_mail_from: sender@example.com
    antirootkits_mail_to: receiver@example.com
    rkhunter: true
    chkrootkit: true
    unhide: true
  roles:
    - role: mablanco.antirootkits
```

## Upgrading from older versions

- **Shell Detector** is no longer supported: it has had no updates since 2019 and needs Python 2. The role fails if `shelldetector` is set to `true`, and removes the files that older versions installed for it.
- **chkrootkit** now uses the daily job of the Debian package, which mails its reports with `MAILTO`. If an older version of the role replaced that job, the role restores it.
- Debian before 12, Ubuntu before 24.04 and EL before 9 are no longer tested.

## Development

The role is tested with [Molecule](https://ansible.readthedocs.io/projects/molecule/) in Docker containers of every supported platform, and linted with [ansible-lint](https://ansible.readthedocs.io/projects/lint/). CI runs both on every change and every week.

```bash
pip install -r requirements-dev.txt
ansible-lint
molecule test --all
```

## License

GPLv3

#software 

Fail2Ban scans log files like `/var/log/auth.log` and bans IP addresses conducting too many failed login attempts. It does this by updating system firewall rules to reject new connections from those IP addresses, for a configurable amount of time. Fail2Ban comes out-of-the-box ready to read many standard log files, such as those for sshd and Apache, and is easily configured to read any log file of your choosing, for any error you wish.

## Installation
I forked the master branch of the [official repository](https://github.com/fail2ban/fail2ban) on GitHub. Once cloned, go to the `fail2ban` directory and run
```
sudo python setup.py install
```
**Note**: Be careful if you are using conda. In this case you have to explicitly specify which python to use so replace `python` with `<path-to-miniconda3>/envs/<env-name>/bin/python3.*`.
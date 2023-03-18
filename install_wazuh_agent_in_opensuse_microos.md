# How to install the wazuh agen in opensuse microos

Opensuse MicroOs has a read only root and works with snaphot.
Wahu agent installs itself in /var/ossec. /opt/ossec would be better and more conforming to file location standards in linux.

To properly install the agent do the following:

1. Create the install directory and set the access.
```
mkdir /var/ossec
chmod o-rwx,g-w /var/ossec
```

2. Go into the transaction shell 
```
transactional-update shell
```

3. Create the bind mount to use for /var/ossec during install and updates.
```
cat > /usr/etc/tukit.conf.d/ossec.conf <<\EOF
BINDDIRS["ossec-dir"]="/var/ossec"
EOF
```
4. exit the transaction shell and reboot
5. After reboot, again go into the transaction shell
6. Import the Wazuh GPG key
```
rpm --import https://packages.wazuh.com/key/GPG-KEY-WAZUH
```

7. Add a repository for wazuh
```
cat > /etc/zypp/repos.d/wazuh.repo <<\EOF
[wazuh]
gpgcheck=1
gpgkey=https://packages.wazuh.com/key/GPG-KEY-WAZUH
enabled=1
name=EL-$releasever - Wazuh
baseurl=https://packages.wazuh.com/4.x/yum/
protect=1
EOF
```

8. refresh the repositories
```
zypper refresh
```

9. install the wazuh agent

```
WAZUH_MANAGER="<ip address or dns name of wazuh manager" zypper install wazuh-agent
```
10. Exit the transaction shell and reboot
11. After the boot enable and start the wazuh agent
```
systemctl enable --now wazuh-agent
```

Follow https://wazuh.com/blog/docker-container-security-monitoring-with-wazuh/ for hints on how to monitor docker containers.




Reference:
https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-linux.html



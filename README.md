## LDAP Client Ansible
### Prerequisites

Ubuntu 20.04 or later

### Installation

Follow these steps to install and configure an LDAP client using Ansible:

##### 1. Install the required packages by running the following commands:

```console
$ sudo apt-get update
$ sudo apt-get install libnss-ldap libpam-ldap ldap-utils nslcd
```

##### 2. Configure the /etc/nslcd.conf file with your LDAP server details, such as the server URL, base DN, and credentials. For example:

```
uri ldap://ldap.example.com
base dc=example,dc=com
binddn cn=admin,dc=example,dc=com
bindpw YOURPASSWORD_HERE
```

##### 3. Configure the /etc/nsswitch.conf file to use LDAP for user and group information. For example:

```
passwd:         files ldap
group:          files ldap
shadow:         files ldap
```

##### 4. Configure PAM to use LDAP authentication in the /etc/pam.d/common-session file. Add the following lines:

```
session optional        pam_mkhomedir.so umask=0022 skel=/etc/skel
session required        pam_unix.so
session optional        pam_ldap.so
```

##### 5. Restart the nslcd service by running the following command:

```console
$ sudo systemctl restart nslcd
```

Verify the configuration by querying for a user or group:

```console
$ getent passwd <username>
$ getent group <groupname>
```
#### NOTE 
  If the getent passwd command does not return any information for the specified user, it could indicate a problem with your LDAP configuration. To troubleshoot, check the /var/log/auth.log file.

Once the LDAP client is installed and configured, you can log in using your LDAP username and password:

```console
$ login <username> 
Password: ****
Welcome to Ubuntu.
```

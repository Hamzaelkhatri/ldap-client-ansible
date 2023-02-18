# LDAP Client Ansible

## install Ldap without ansible 

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

---

## install ldap client using ansible 

Change the file [playbook](./playbook.yml)

```yml
vars: 
    ldap_server: [URL]
    ldap_base: [LDAP_BASE]
    ldap_binddn: [BINDDN_ROOT]
    ldap_bindpw: [PASSWORD]
    ldap_version: [LDAP_VERSION]
    ldap_pam_password: [PAM_PASSWPORD_CRYPT]
```

Example :

```yml
vars:
    ldap_server: ldap://192.168.8.149
    ldap_base: dc=example,dc=com
    ldap_binddn: cn=admin,dc=example,dc=com
    ldap_bindpw: 1234
    ldap_version: 3
    ldap_pam_password: md5
```

#### Note 
you must change also `remote_user` with your username 

put the clients ip in [hosts](./inventory/all)
```
[all]
Client Ips
```

Example :

```
[all]
192.168.8.168
192.168.8.164
192.168.8.163
192.168.8.161
```

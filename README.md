Instructions for setting up integrated authentication via LDAP for Ubuntu 16.04.

Install dependencies:

    sudo apt install -y ldap-utils libpam-ldap libnss-ldap nscd

Backup default configuration files:

    sudo mv /etc/ldap.conf /etc/ldap.conf.bak
    sudo mv /etc/ldap/ldap.conf /etc/ldap/ldap.conf.bak

Create `/etc/ldap.conf` and `/etc/ldap/ldap.conf` with these contents:

      URI ldaps://ds.uark.edu/
      BASE ou=people,dc=uark,dc=edu
      tls_cacertfile /etc/ssl/certs/ca-certificates.crt

Edit `/etc/nsswitch.conf` and change these 3 lines:

      passwd:         compat
      group:          compat
      shadow:         compat

by adding `ldap` to the end like this:

      passwd:         compat ldap
      group:          compat ldap
      shadow:         compat ldap

Now restart the service:

    /etc/init.d/nscd restart

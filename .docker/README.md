# Hive instances

Here you may find dockerized hive configurations.

## Prerequisites

You should use a *nix system with pre-installed:

- docker

- docker-compose

- krb5-user (for Debian) # in case you want use kerberized Hive

- Open JDK

## Pre-install

Firstly, you should build images and configure certs:

```bash
make build

make generate_ssl
```

## Usage

To use Hive instance you should run:

```bash
make up TYPE=<type> # run

make down # stop

make log # watch hive-server logs
```

where &lt;type&gt; is the suffix of hive-site.xml configs in the folder confs/.

Example:

```bash
make up TYPE=tcp.nosasl
```

To run kerberized Hive instance run:

```bash
make up-krb TYPE=tcp.kerberos

kinit hive@KERBEROS.SERVER

password: hive
```

To stop instance:

## Set up kerberos client

To use kerberized Hive you should install and set up krb5 client.

For Ubuntu you should install krb5-user:

```bash
sudo apt install krb5-user
```

Then add to `/etc/hosts`

```
127.0.0.1 KERBEROS.SERVER
127.0.0.1 kerberos.server
```

And add to `/etc/krb5.conf` next configuration:

```
[libdefaults]
    default_realm = KERBEROS.SERVER
[realms]
    KERBEROS.SERVER = {
        kdc = kerberos.server
        admin_server = kerberos.server
    }
[domain_realm]
    .kerberos.server = KERBEROS.SERVER
    kerberos.server = KERBEROS.SERVER
```

Afteward you may obtain a ticket from dockerized Kerberos server:

```bash
make kinit
```

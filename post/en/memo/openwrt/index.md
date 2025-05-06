# OpenWRT


OpenWRT

## Contents

1. [Wildcard Domain&Subdomain](#wildcard-domain-&-subdomain)

## Wildcard Domain & Subdomain

### Introduction

The IP address can be fixed manually in **Network -> DHCP and DNS -> Static Leases**.
The default local domain will be automatically allocated by hostname.
For instance, **ubuntuNginx.lan** is the local domain of **ubuntuNginx** machine.

However, if I want to all sub-domains pointing to a ip address, it has to add hostnames into **Network -> Hostnames** with every sub-domain manually.

### Wildcard match

There is not a general solution in LUCI UI. It requires operating in ssh or console.

Add the line, in the **/etc/dnsmasq.conf**:

```bash
# '.' is required
address=/.[domain]/[ip address]
```

Example:

```bash
# '.' is required
address=/.ubuntunginx.lan/192.168.0.2
```


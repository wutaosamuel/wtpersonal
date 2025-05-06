# OpenWRT IPV6 Setup


TODO: Add ipv6 setting up for home network

Enable IPV6 connection in OpenWRT.

## IPV6 network

1. ipv6 relay mode if router does not been assigned ipv6 address with prefix delegation.
2. ipv6 server mode if router has ipv6 address with prefix delegation.

## IPV6 problem in relay mode

1. if upstream router has wrong settings that cause all clients cannot be assigned ipv6 address.
It requires client to ping the router for getting ipv6 address.
    - TODO: add keywords for searching the solutions
2. connection problem if router sets learn routes in v24.01
    - disable learn routes in WAN interface
    - disable learn routes in LAN interface


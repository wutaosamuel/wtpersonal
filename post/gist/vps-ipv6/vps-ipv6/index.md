# Connect to IPV6 network with public-ipv4-only VPS


## Background

There are many cloud service providers only sell public-ipv4-only virtual machines. But, for some reasons, having ipv6 support could make things easier. In this case, we'll use ipv4 to ipv6 tunnel to connect to ipv6 network.

## Step

1. register in [HE.net tunnel broker](https://tunnelbroker.net) for free to use ipv4ToIpv6 tunnel service
    1.1. create tunnel
    1.2. configure ipv6 tunnel on virtual machine through its example configurations.
2. waiting until it is ready

## IPV6 tunnel problem

1. He.net unable to register by gmail and outlook.
    - Solution: it works with 163, registered at 2024-12-21.
2. He.net cannot create ipv6 tunnel, caused by "This network is restricted"
    - Possible reason: He.net block target ip address
    - Solution: create tunnel with valid ipv4 address. Then, switch to blocked ip address, created at 2024-12-21
3. He.net tunnel will close due to inactivity for a certain time.
    - Solution: ping a ipv6 website periodically every 10 minutes.
    Sh: `crontab -l 2>/dev/null | echo "*/10 * * * * ping -c 2 ipv6.google.com" | crontab -`


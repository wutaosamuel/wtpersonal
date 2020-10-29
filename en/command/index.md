# Unix-like System Commands


Record infrequently used but potential useful commands

## Contents

1. [iperf3 - Network speed test](#iperf3)

## iperf3

Test network speed between devices.
First, open server-side iperf/iperf3 service,
then, test it by client-side iperf/iperf3 application.
Under windows system, the application is iperf3.exe.

### Open server-side iperf3 service

```bash
iperf3 -s -p 5201
```

### Open client-side iperf3 service

```bash
iperf3 -c [ip address] -p 5201
```


# Unix-like System Commands


Record infrequently used but potential useful commands

## Contents

1. [iperf3 - Network speed test](#iperf3)
2. [qemu - convert images](#qemu-convert-images)
3. [base64 - encode & decode](#base64-encode-and-decode)
4. [smartctl - check macos ssd health](#macos-check-ssd-health)
5. [git-crypt - transparent file encryption in git](#git-crypt)

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

## Qemu convert images

```bash
qemu-img convert -f [format] -O [output format] vm-114-disk-0.qcow2 vm-114-disk-0.raw
```

Example:

```bash
# convert qcow2 to raw image
qemu-img convert -f qcow2 -O raw vm-114-disk-0.qcow2 vm-114-disk-0.raw
```

## base64 encode and decode

encode:

```bash
echo 'YourText' | base64
```

create pseudorandom number and encrypted by base64:

```bash
head -c32 /dev/urandom | base64
```

decode:

```bash
echo 'WW91clRleHQK' | base64 --decode
```

## macos check ssd health

```bash
smartctl -a [device id]
```

## zfs send entire pool

```bash
zfs snap pool_old@migrate
zfs send -Rv pool_old@migrate | zfs receive -F -d pool_new
```

## zfs send dataset

```bash
zfs snap pool/dataset@migrate
zfs send -R pool/dataset@migrate | zfs receive -F pool/dataset_new
```

## git-crypt

## git-crypt initialisation

Initialise to automatically create .git-crypt folder

```bash
git-crypt init
```

Add public gpg key to project

```bash
git-crypt add-gpg-user --trusted [your_email_or_key_id_or_fringerprint]
```

example:

```bash
git-crypt add-gpg-user --trusted F278A3CCB6C173BD3660D173564D16856D20F3FC
```

Select files by adding .gitattributes file to automatic encrypt

```git config
# Encrypt
## Encrypt all files under vault
vault/** filter=git-crypt diff=git-crypt

## Make sure that .gitattributes is never encrypted.
.gitattributes !filter !diff
## Make sure that .gitignore is never encrypted.
.gitignore !filter !diff
```

## common git-crypt usage

- Only show the files required to encrypt

```bash
git-crypt status -e
```

- Remedies if git commit before encrypting

```bash
git-crypt status -f
```


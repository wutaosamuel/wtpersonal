# Yubikey and backup yubikey used as Smart Card with GnuPG


## Install Software

### Ubuntu

```bash
# TODO: apt install software 
```

### Windows

```bash
choco install git wget
# TODO: install yubikey-personalization ykman
# Only uses gpg to operate yubikey as smart card. Not test for configuring yubikey
```

### macOS

```bash
brew install \
  gnupg yubikey-personalization ykman pinentry-mac wget
```

## Yubikey GnuPG

The instructions are mainly referred by [YubiKey-Guide](https://github.com/drduh/YubiKey-Guide). I simplify the process of configuring yubikey, but sacrifice the security.

Create a temporary directory

```bash
GNUPGHOME=$(mktemp -d -t gnupg-$(date +%Y-%m-%d)-XXXXXXXXXX)
```

Download hardened configuration

```bash
cd $GNUPGHOME
wget https://raw.githubusercontent.com/drduh/config/master/gpg.conf
```

Set identity

```bash
IDENTITY="name <email>"
```

Set key type as 4096-bit RSA

```bash
KEY_TYPE=rsa4096
```

Set expiration 10 years

```bash
EXPIRATION=10y
```

Generate strong passphrase

```bash
CERTIFY_PASS=$(LC_ALL=C tr -dc 'A-Z1-9' < /dev/urandom | \
  tr -d "1IOS5U" | fold -w 30 | sed "-es/./ /"{1..26..5} | \
  cut -c2- | tr " " "-" | head -1) ; echo "$CERTIFY_PASS"
```

Save to safe place

```bash
echo "Warning: Save To Save Place"
```

Create certify key with no expiration date

```bash
gpg --batch --passphrase "$CERTIFY_PASS" \
    --quick-generate-key "$IDENTITY" "$KEY_TYPE" cert never
```

Save revocation certification

```bash
# TODO: paraphrase stdout, then save to variable for auto backing up
cp /Users/[user name]/.gnupg/openpgp-revocs.d/[name].rev
```

Get certify key identifier and fingerprint

```bash
KEYID=$(gpg -k --with-colons "$IDENTITY" | awk -F: '/^pub:/ { print $5; exit }')

KEYFP=$(gpg -k --with-colons "$IDENTITY" | awk -F: '/^fpr:/ { print $10; exit }')

printf "\nKey ID: %40s\nKey FP: %40s\n\n" "$KEYID" "$KEYFP"
```

Create 3 subkeys, consist of 1 signature keys, 1 encryption keys and 1 authentication keys.

```bash
for SUBKEY in sign encrypt auth ; do \
  gpg --batch --pinentry-mode=loopback --passphrase "$CERTIFY_PASS" \
      --quick-add-key "$KEYFP" "$KEY_TYPE" "$SUBKEY" "$EXPIRATION"
done
```

Verify keys

```bash
gpg -K
```

Edit public key, to avoid uploading issue of keybase

```bash
gpg --edit-key $KEYID
```

gpg> setpref AES256 AES192 AES 3DES SHA512 SHA384 SHA256 SHA224 SHA1 ZLIB BZIP2 ZIP
```bash
setpref AES256 AES192 AES 3DES SHA512 SHA384 SHA256 SHA224 SHA1 ZLIB BZIP2 ZIP
```

Backup keys for certify key, subkeys and public key

```bash
gpg --output $GNUPGHOME/$KEYID-Certify.key \
    --batch --pinentry-mode=loopback --passphrase "$CERTIFY_PASS" \
    --armor --export-secret-keys $KEYID

gpg --output $GNUPGHOME/$KEYID-Subkeys.key \
    --batch --pinentry-mode=loopback --passphrase "$CERTIFY_PASS" \
    --armor --export-secret-subkeys $KEYID

gpg --output $GNUPGHOME/$KEYID-$(date +%F).asc \
    --armor --export $KEYID
```

Save back-up files to safe place or offline. I save the files to the folder where only root have permissions to read and write.

```bash
echo "Warning: Save the backup files to save place"
```

Reset your Yubikey if you want

```bash
ykman openpgp reset
```

Set Admin Pin

```bash
ADMIN_PIN="<your pin>"
```

Set User Pin

```bash
USER_PIN="<your pin>"
```

Change Admin Pin and User Pin (default: 12345678 and 123456 separately)

```bash
gpg --card-edit
```

gpg/card> admin

```bash
admin
```

gpg/card> passwd

```bash
passwd
```

Change the number of retry attempts

```bash
ykman openpgp access set-retries 5 5 5 -f -a $ADMIN_PIN
```

Set Yubikey login information

```bash
gpg --command-fd=0 --pinentry-mode=loopback --edit-card <<EOF
admin
login
$IDENTITY
$ADMIN_PIN
quit
EOF
```

Set Yubikey attributes

```bash
gpg --card-edit
```

gpg/card> admin

```bash
admin
```

Name, gpg/card> name

```bash
name
```

Language, gpg/card> lang

```bash
lang
```

URL, gpg/card> url

```bash
url
```

Sex, gpg/card> sex

```bash
sex
```

Transfer subkeys to Yubikey

Transfer signature key to Yubikey key slot 1

```bash
gpg --edit-key $KEYID
```

select first ssb

```bash
gpg> key 1

sec  rsa4096/0x4ECBB9CD3E2CC0A7
     created: 2024-04-16  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb* rsa4096/0xD625518597D8F9CA
     created: 2024-04-16  expires: 2034-04-14  usage: S   
ssb  rsa4096/0x305EB972E4B47129
     created: 2024-04-16  expires: 2034-04-14  usage: E   
ssb  rsa4096/0x0AB127F2342DA650
     created: 2024-04-16  expires: 2034-04-14  usage: A   
[ultimate] (1). test <test@email.test>
```

save to yubikey slot 1

```bash
gpg> keytocard
Please select where to store the key:
   (1) Signature key
   (3) Authentication key
Your selection? 1

sec  rsa4096/0x4ECBB9CD3E2CC0A7
     created: 2024-04-16  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb* rsa4096/0xD625518597D8F9CA
     created: 2024-04-16  expires: 2034-04-14  usage: S   
ssb  rsa4096/0x305EB972E4B47129
     created: 2024-04-16  expires: 2034-04-14  usage: E   
ssb  rsa4096/0x0AB127F2342DA650
     created: 2024-04-16  expires: 2034-04-14  usage: A   
ssb  rsa4096/0x4CB21BA40EBBC385
[ultimate] (1). test <test@email.test>

gpg> save
```

Repeat process for Key 2 and 3.
Or save three subkeys into yubikey at once.

```bash
gpg> key 1

sec  rsa4096/0x4ECBB9CD3E2CC0A7
     created: 2024-04-16  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb* rsa4096/0xD625518597D8F9CA
     created: 2024-04-16  expires: 2034-04-14  usage: S   
     card-no: 0006 20572146
ssb  rsa4096/0x305EB972E4B47129
     created: 2024-04-16  expires: 2034-04-14  usage: E   
     card-no: 0006 20572146
ssb  rsa4096/0x0AB127F2342DA650
     created: 2024-04-16  expires: 2034-04-14  usage: A   
     card-no: 0006 20572146
[ultimate] (1). test <test@email.test>

gpg> keytocard
Please select where to store the key:
   (1) Signature key
   (3) Authentication key
Your selection? 1

sec  rsa4096/0x4ECBB9CD3E2CC0A7
     created: 2024-04-16  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb* rsa4096/0xD625518597D8F9CA
     created: 2024-04-16  expires: 2034-04-14  usage: S   
     card-no: 0006 20572146
ssb  rsa4096/0x305EB972E4B47129
     created: 2024-04-16  expires: 2034-04-14  usage: E   
     card-no: 0006 20572146
ssb  rsa4096/0x0AB127F2342DA650
     created: 2024-04-16  expires: 2034-04-14  usage: A   
     card-no: 0006 20572146
[ultimate] (1). test <test@email.test>

Note: the local copy of the secret key will only be deleted with "save".
gpg> key 1

sec  rsa4096/0x4ECBB9CD3E2CC0A7
     created: 2024-04-16  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  rsa4096/0xD625518597D8F9CA
     created: 2024-04-16  expires: 2034-04-14  usage: S   
     card-no: 0006 20572146
ssb  rsa4096/0x305EB972E4B47129
     created: 2024-04-16  expires: 2034-04-14  usage: E   
     card-no: 0006 20572146
ssb  rsa4096/0x0AB127F2342DA650
     created: 2024-04-16  expires: 2034-04-14  usage: A   
     card-no: 0006 20572146
[ultimate] (1). test <test@email.test>

gpg> key 2

sec  rsa4096/0x4ECBB9CD3E2CC0A7
     created: 2024-04-16  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  rsa4096/0xD625518597D8F9CA
     created: 2024-04-16  expires: 2034-04-14  usage: S   
     card-no: 0006 20572146
ssb* rsa4096/0x305EB972E4B47129
     created: 2024-04-16  expires: 2034-04-14  usage: E   
     card-no: 0006 20572146
ssb  rsa4096/0x0AB127F2342DA650
     created: 2024-04-16  expires: 2034-04-14  usage: A   
     card-no: 0006 20572146
[ultimate] (1). test <test@email.test>

gpg> keytocard
Please select where to store the key:
   (2) Encryption key
Your selection? 2

sec  rsa4096/0x4ECBB9CD3E2CC0A7
     created: 2024-04-16  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  rsa4096/0xD625518597D8F9CA
     created: 2024-04-16  expires: 2034-04-14  usage: S   
     card-no: 0006 20572146
ssb* rsa4096/0x305EB972E4B47129
     created: 2024-04-16  expires: 2034-04-14  usage: E   
     card-no: 0006 20572146
ssb  rsa4096/0x0AB127F2342DA650
     created: 2024-04-16  expires: 2034-04-14  usage: A   
     card-no: 0006 20572146
[ultimate] (1). test <test@email.test>

Note: the local copy of the secret key will only be deleted with "save".
gpg> key 2

sec  rsa4096/0x4ECBB9CD3E2CC0A7
     created: 2024-04-16  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  rsa4096/0xD625518597D8F9CA
     created: 2024-04-16  expires: 2034-04-14  usage: S   
     card-no: 0006 20572146
ssb  rsa4096/0x305EB972E4B47129
     created: 2024-04-16  expires: 2034-04-14  usage: E   
     card-no: 0006 20572146
ssb  rsa4096/0x0AB127F2342DA650
     created: 2024-04-16  expires: 2034-04-14  usage: A   
     card-no: 0006 20572146
[ultimate] (1). test <test@email.test>

gpg> key 3

sec  rsa4096/0x4ECBB9CD3E2CC0A7
     created: 2024-04-16  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  rsa4096/0xD625518597D8F9CA
     created: 2024-04-16  expires: 2034-04-14  usage: S   
     card-no: 0006 20572146
ssb  rsa4096/0x305EB972E4B47129
     created: 2024-04-16  expires: 2034-04-14  usage: E   
     card-no: 0006 20572146
ssb* rsa4096/0x0AB127F2342DA650
     created: 2024-04-16  expires: 2034-04-14  usage: A   
     card-no: 0006 20572146
[ultimate] (1). test <test@email.test>

gpg> keytocard
Please select where to store the key:
   (3) Authentication key
Your selection? 3

sec  rsa4096/0x4ECBB9CD3E2CC0A7
     created: 2024-04-16  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  rsa4096/0xD625518597D8F9CA
     created: 2024-04-16  expires: 2034-04-14  usage: S   
     card-no: 0006 20572146
ssb  rsa4096/0x305EB972E4B47129
     created: 2024-04-16  expires: 2034-04-14  usage: E   
     card-no: 0006 20572146
ssb* rsa4096/0x0AB127F2342DA650
     created: 2024-04-16  expires: 2034-04-14  usage: A   
     card-no: 0006 20572146
[ultimate] (1). test <test@email.test>

Note: the local copy of the secret key will only be deleted with "save".
gpg> save
```

Option: Set touch for encryption operation

```bash
ykman openpgp keys set-touch dec on
```

Option: Set touch for signature operation

```bash
ykman openpgp keys set-touch sig on
```

Option: Set touch for authentication operation

```bash
ykman openpgp keys set-touch aut on
```

Option: Verify touch policy by UIF setting

```bash
gpg --card-status
```

Delete secret and public keys

```bash
gpg --delete-secret-keys $KEYID
gpg --delete-keys $KEYID
```

Import backup keys, because the subkeys have been deleted once the keys save to yubikey. It requires to recover the subkeys.

```bash
gpg --import $GNUPGHOME/$KEYID-Certify.key
gpg --import $GNUPGHOME/$KEYID-Subkeys.key
```

Insert second yubikey and repeat the process of inserting 3 subkeys into it

```bash
echo "Repeat the steps of importing the key"
gpg --edit-key $KEYID
```

## Using Yubikey

Initialize GnuPG

```bash
gpg -k
```

Import a hardened configuration

```bash 
cd ~/.gnupg

wget https://raw.githubusercontent.com/drduh/config/master/gpg.conf
```

Set option, avoiding the problem that GnuPG will repeatedly prompt for the insertion of an already-inserted Yubikey

```bash
touch scdaemon.conf

echo "disable-ccid" >>scdaemon.conf
```

Import public key

```bash
KEYID=[your public key ID]
```

```bash
gpg --import [somewhere]/$KEYID*.asc
```

Encryption

```bash
mkdir ~/tmp
cd ~/tmp
```

```bash
echo "\ntest message string" | \
  gpg --encrypt --armor \
      --recipient $KEYID --output encrypted.txt
```

Decryption

```bash
gpg -o plaintxt.txt --decrypt --armor encrypted.txt
```

Verify result

```bash
cat plaintxt.txt
rm plaintxt.txt
```

Checkout backup Yubilikey with inserting bakcup yubikey. Then, remove the GnuPG shadowed key.
Because the stub is stored the GnuPG key ID and yubikey serial number. It requires to recreate the correct stub.
Finally, repeat encryption and decryption steps.

```bash
gpg-connect-agent "scd serialno" "learn --force" /bye
```



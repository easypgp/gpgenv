# GPGenv

Create and manage GPG environments.

## Installation

```bash
$ wget https://github.com/easypgp/gpgenv/releases/latest/download/gpgenv
```

Then source the script in your shell profile.

```bash
# .bash_profile
source /path/to/gpgenv
```

## Usage

```bash
$ source gpgpenv
$ gpgpenv help

Manage multiple gnupg environments.
version: latest

 Usage: gpgenv [command]


 Exemples:
       gpgenv help
       gpgenv use /tmp/gnupg
       gpgenv use
       gpgenv cd
       gpgenv create
       gpgenv create /tmp/gnupg
       gpgenv current
       gpgenv backup /media/secret-store/gnupg-$(date  +%Y-%m-%d).tgz
       gpgenv restore /media/secret-store/gnupg-2024-06-18.tgz
       gpgenv deactivate
```

## Example Usage

### Manage Certification Key Offline

To secure your key, it is recommended to keep your certification key offline.
This means that you need to import and then delete your certification key every
time you need to update your key or sign a new key.

`gpgenv` makes this process easier by allowing you to create a backup and
restore your GPG environment.

First create a new GPG environment.

```bash
$ gpgenv create
$ gpgp --full-generate-key
```

Then backup your GPG environment.

```bash
$ gpgenv backup /media/secret-store/gnupg-$(date +%Y-%m-%d).tgz
$ gpgenv deactivate
```

### Restore Certification Key

To restore your certification key, you need to restore your GPG environment.

```bash
$ gpgenv restore /media/secret-store/gnupg-2024-06-18.tgz
# Restored backup in /tmp/gnupg-some-uuid
# To activate the environment run:
#     gpgenv use /tmp/gnupg-some-uuid
$ gpgenv use /tmp/gnupg-some-uuid
```

Then do the work needing the certification key.
When it is done, backup the GPG environment and deactivate it.

```bash
$ gpg --export --armor KEYID > /media/secret-store/KEYID.pub.asc
$ gpg --export --export-secret-subkeys --armor KEYID > /media/secret-store/KEYID.subkeys.asc
$ gpgenv backup /media/secret-store/gnupg-$(date +%Y-%m-%d).tgz
$ gpgenv deactivate
```

And finally import the new keys in your main GPG environment.

```bash
$ gpg --import /media/secret-store/KEYID.pub.asc
$ gpg --allow-secret-key-import --import /media/secret-store/KEYID.subkeys.asc
```

## License

[X11 License](https://spdx.org/licenses/X11.html)

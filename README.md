# perfectcrypt

Installing this package allows you to set up a dedicated 'kill password' 
that can be used to wipe the encryption keys needed to unlock your encrypted 
partitions. This password can be entered at the early boot prompt, 
where you normally type the passphrase to access the encrypted drive(s).

## How I can add the kill password

After having installed the package, run “dpkg-reconfigure
perfectcrypt”. Behind the scene, this creates
`/etc/perfectcrypt/password_hash` with
the output of `echo your-password |
/usr/lib/perfectcrypt/crypt --generate` and rebuilds
the initramfs (`update-initramfs -u`).

After installing the package, simply run dpkg-reconfigure perfectcrypt. 
This will automatically:
Generate /etc/perfectcrypt/password_hash using echo your-password | /usr/lib/perfectcrypt/crypt --generate
Update the initramfs by running update-initramfs -u in the background.

## How does it work?

The package replaces /lib/cryptsetup/askpass with a custom script. 
This script calls the original tool to retrieve the password 
but performs additional processing before outputting the same password to stdout.

To determine which partition is being unlocked, we use the environment 
variables set by /usr/share/initramfs-tools/scripts/local-top/cryptroot.

For flush the encryption keys, we execute cryptsetup erase <device>.

In file /etc/perfectcrypt/settings.cfg, you can edit variables:
1. failsleep - how many seconds will cryptsetup sleep after 
entering password 3 times incorrectly
2. reaction_erase - how will cryptsetup behave if the system 
volume encryption keys are cleared
3. show_author - will information about the developer and system
name be displayed at the password request stage
4. erase_killed_disks - Will automatic disk cleanup be started
after key slots are cleared
5. erase_disk_1, erase_disk_2, erase_disk_3, erase_disk_4 - Specify
the uuids of the disks whose key slots will also be cleared (if necessary).
By default - none.

## Backup encrypt skeys

WARNING!
If you do not back up the LUKS header, you may permanently lose access to all encrypted data in the event of any failure or accidental damage.

What happens if the LUKS header is damaged?

All your data will become INACCEPTABLE.
Even if you know the correct password, you will not be able to decrypt the disk.

No recovery software will help - without the header, the LUKS volume turns into a useless set of bytes.

Why do you MUST do this NOW?
One disk failure, random error or metadata corruption - your data will be lost forever.
Entering the "kill" password perfectcrypt WILL DESTROY HEADER!

WARNING: IF YOU IGNORE THIS MESSAGE, YOU KNOWINGLY RISK LOSS OF ALL DATA.
THE PACKAGE DEVELOPERS WILL NOT BE ABLE TO HELP YOU RECOVER YOUR DATA!

Here's the command to use (replace `<device>` with the path of the device
file representing your luks encrypted partition, and `<your-backup-file>`
with the path to the backup file to create):
```
$ sudo cryptsetup luksHeaderBackup <device> --header-backup-file <your-backup-file>
```

To later restore the header, you will have to do:
```
$ sudo cryptsetup luksHeaderRestore <device> --header-backup-file <your-backup-file>
```



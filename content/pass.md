+++
date = "Thu Mar 3 14:00:00 CET 2016"
title = "Install Pass on different machines with gpg"

+++

# Using Pass + gpg on different machines #

Piouf, as my first introduction to gpg, that was more complicated than expected.
Pass is a password manager that uses gpg to encrypt your password and can be
intergrated with Git. So as long as you have the same gpg key on your devices,
you can have the passwords in clear text AND being able to push those encrypted
password on any git repo. That's cool !

### Create key ###

`gpg --gen-key` (it can depends on your distro)
Choose (1) Rsa & Rsa with 4096 bits (at least)

### Export key to remote server ###

Notes from https://www.debuntu.org/how-to-importexport-gpg-key-pair/

* Lookup the id of your key: `gpg --list-keys`
* Export public key: `gpg --output mygpgkey_pub.gpg --armor --export <key-id>` where key-id is the
  last part when you list the key `*****/<key-id>`
* Export secret key : `gpg --output mygpgkey_sec.gpg --armor --export-secret-key ******`
* Copy keys to remote server: `scp mygpgkey_pub.gpg mygpgkey_sec.gpg user@remotehost:~/`
* Connect to remote: `ssh user@remotehost`
* Import the public key: `gpg --import ~/mygpgkey_pub.gpg`
* Import the secret key: `gpg --allow-secret-key-import --import
  ~/mygpgkey_sec.gpg`

## Troubles ##

I've had trouble when importing the key because of pinentry. In Archlinux,
pinentry points to `/usr/bin/pintentry-gtk` (why ...?). You have to delete the
symlink `/usr/bin/pinentry` and to point it to whatever (ncurse, or tty if you
work on a remote server)

## Pass manager ##

At this point you can use the Pass manager with the key you created:
```
pass init <mykey-id>
pass git init
```

Then register one password to test if it works
```
pass insert server/test
```

If it *fails* with `There is no assurance this key belongs to the named user`,
that mean you have to put a higher level of trust in the key you just imported.
To do that:
```
gpg --edit-key <mykey-id>
trust
5 (maximum level since it is basically my key)
quit
```
Then you can try again to register the password and it should works !

## Different machines ##

Now on the remote server you have a git repo in `$HOME/.password_store`. If you
want your passwords on another machines, make sure you import the key as before.
Then you simply can do:
`cd && git clone user@myserver.com:~/.password-store`
Then your local *pass* will simply look at this file.


Happy password managing ><





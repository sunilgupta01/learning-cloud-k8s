## proper permissions for authorized_keys so that one can do SSH

copy public key to other machines authorized keys

```
scp /home/user1/.ssh/id_ed25519.pub remoteuser@remotehost:/home/remoteuser/.ssh/authorized_keys
```

```
chmod go-w ~/
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

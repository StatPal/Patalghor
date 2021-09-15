# SSH - connecting and hosting
Gen. profociency: Beginner-Intermediate


## Connecting
Basic part


## Hosting
Intermediate part

We will use `openssh` for this whole procedure. SSH has two components: the command one use on the local machine to start a connection, and a server to accept incoming connection requests. 
One way of checking that you can actally do this, i.e., you have everything installed with proper permission:
```bash
file /etc/ssh/ssh_config
# and
file /etc/ssh/sshd_config
```
If everything is okay, you would get something like: `<filename>: ASCII text`

Should this return a `No such file` or `directory error`, then you don't have the SSH command installed. If you get `regular file, no read permission`, then you don't have the proper permissions. 

On the **remote computer, enable SSH serive with systemd:**

```bash
sudo systemctl enable --now sshd
```


To SSH into the remote computer, you must know its internet protocol (IP) address or its resolvable hostname. To find the remote machine's IP address, use the ip command (on the remote computer):
```bash
ip addr show | grep "inet "
```

However, if the remote computer is not in the same network, you need more works. 


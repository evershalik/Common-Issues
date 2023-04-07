### Problem:Kernel module issue
![image](https://i.stack.imgur.com/nIxI6.png)

### Solution:


If you installed VirtualBox by official manual and didn't forget to delete installed one from default Ubuntu repository.

Check if virtualbox-dkms is installed:
```
dpkg -l | grep virtualbox-dkms
```

If yes, then delete it and install dkms

```
sudo apt-get purge virtualbox-dkms && \
sudo apt-get install dkms
```

Rebuild VirtualBox kernel modules:
```
sudo /sbin/vboxconfig 
```


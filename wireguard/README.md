# Fixing Internet issue on wg-quick up cmd.

##### If your internet is not working after using wg-quick up <filename.conf>, then follow thses basic steps to resolve this issue.

#### Step 1: Disable UFW(Uncomplicated Firewall):
`> ufw disable`
#### Step 2. Shut down < wg_file > before updating its config file.
`wg-quick down <wg_file_name>`

#### Step 3. Add in new UFW rules into the config file
Go to `/etc/wireguard/wg0.conf` and edit the file, append these commands to the back of PostUp and PostDown. Replace `<port>` with the Wireguard listen port that you set up:

> PostUp = ...; ufw route allow in on wg0 out on eth0; ufw route allow in on eth0 out on wg0; ufw allow proto udp from any to any port <port>PostDown = ...; ufw route delete allow in on wg0 out on eth0; ufw route delete allow in on eth0 out on wg0; ufw delete allow proto udp from any to any port <port>;


After this your wg_file will look like:
![image](https://user-images.githubusercontent.com/98207888/203267538-b084652c-731f-4f4b-93fd-21932a4fc5df.png)


#### Step 4. Make sure the server allows IP Forwarding

Open `/etc/sysctl.conf` and uncomment these lines:
```
net.ipv4.ip_forward=1  
net.ipv6.conf.all.forwarding=1
```

Open  `/etc/ufw/sysctl.conf`and uncomment these lines:

```
net/ipv4/ip_forward=1  
net/ipv6/conf/default/forwarding=1  
net/ipv6/conf/all/forwarding=1
```
Then run the command to update the system:

`> sysctl -p`

####  Step 5. Time of reckoning…
Let’s restart UFW:
```
> ufw disable 
> ufw enable
```

Next we get  `wg_file`  back online:
`> wg-quick up wg_file_name`

Then, let’s see if you have the same rules in your UFW (or you can jump right into testing your connection):
`> ufw status`
![image](https://user-images.githubusercontent.com/98207888/203267429-6540e4ad-91cc-46bb-98a7-0c6e5d58e627.png)

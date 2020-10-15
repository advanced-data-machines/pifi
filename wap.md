## Step 6: Set up traffic forwarding

```
sudo vim /etc/sysctl.conf
```

Now find this line:
```
#net.ipv4.ip_forward=1
```

…and remove the comment so that it reads:

```
net.ipv4.ip_forward=1
```


## Step 7: Add a new iptables rule
Next, we’re going to add IP masquerading for outbound traffic on eth0 using iptables:

```
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

save the new iptables rule:

```
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
```

To load the rule on boot, we need to edit the file /etc/rc.local and add the following line just above the line exit 0:

```
iptables-restore < /etc/iptables.ipv4.nat
```


## Step 8: Enable internet connection
Now the Raspberry Pi is acting as an access point to which other devices can connect. However, those devices can’t use the Pi to access the internet just yet. To make the possible, we need to build a bridge that will pass all traffic between the wlan0 and eth0 interfaces.

To build the bridge, let’s install one more package:

```
sudo apt-get install bridge-utils
```

We’re ready to add a new bridge (called br0):

```
sudo brctl addbr br0
```

Next, we’ll connect the eth0 interface to our bridge:

```
sudo brctl addif br0 eth0
```

Finally, let’s edit the interfaces file:

```
sudo vim /etc/network/interfaces
```

…and add the following lines at the end of the file:

auto br0
iface br0 inet manual
bridge_ports eth0 wlan0

## Step 9: Reboot


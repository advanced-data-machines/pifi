# How to use the WiFi on your Raspberry Pi to broadcast a provisioning network

cat /etc/os-releases

## Prerequisites

```
Raspbian Buster

hostapd v2.8-devel
dnsmasq v2.80 
```

## Step 1: Update your OS (recommended)

```
sudo apt-get update
sudo apt-get upgrade   
```

## Step 2: Install hostapd & dnsmasq
```
sudo apt install hostapd
sudo apt install dnsmasq
```

```
sudo systemctl stop hostapd
sudo systemctl stop dnsmasq
```

## Step 3: Configure static IP for wlan0 interface
```sudo vim /etc/dhcpcd.conf```

Now that you’re in the file, add the following lines at the end:

```
interface wlan0
static ip_address=192.168.51.50/24
denyinterfaces eth0
denyinterfaces wlan0
```

*The IP address ```192.168.51.50``` is recommended as tribute to Van Halen and one of the greatest LP's of all time.. RIP Eddie*



## Step 4: Configure DHCP Server

```
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
sudo vim /etc/dnsmasq.conf
```

You’ll be editing a new file now, and with the old one renamed, this is the config file that dnsmasq will use. Type these lines into your new configuration file:

```
interface=wlan0
  dhcp-range=192.168.0.11,192.168.0.30,255.255.255.0,24h
```


## Step 5: Configure Host Access Point Daemon Software

Copy the file located in [workingdir]/pifi/etc/hostapd/hostapd.conf to /etc/hostapd/hostapd.conf

```
cp [workingdir]/pifi/etc/hostapd/hostapd.conf /etc/hostapd/hostapd.conf
```

Here you in the hostapd.conf you can configure your own SSID and passphrase with your own values. This is how you’ll join the Pi’s network from other devices.

Lastly, we still have to show the system the location of the configuration file:

Replace the file hostapd file located in /etc/default/hostapd with the one included in this project.  (make a back up of the original if you feel so compelled to do so)

```
mv  /etc/default/hostapd /etc/default/hostapd.orig
cp [workingdir]/pifi/etc/default/hostapd /etc/default/hostapd
```

At this point you should be able to connect to the Wifi network that will broadcasting from you Raspberry Pi

At this point  you will not be able to pass traffic but you can now run a web server or ssh into your IoT device and configure it etc.

If you need to forward traffic continue reading below


[Configure for use as a Wireless Access Point](wap.md)

## Setup the Raspberry Pi

### Prepare the boot disk

#### Step 1: Create a bootable image

In a shell move to folder `./bin`

```shell
diskutil list

diskutil unmountDisk /dev/disk2

sudo diskutil eraseDisk FAT32 FOO MBRFormat /dev/disk2

sudo dd bs=1m if=2020-02-13-raspbian-buster-lite.img of=/dev/disk2 conv=sync

diskutil unmountDisk /dev/disk2
```

Note: the above applies to flashing the image on a Mac!

The latest `raspian-buster-lite` image can be downloaded from the [Raspian download site](https://www.raspberrypi.org/downloads/raspbian/).


#### Step 2: Setup Wifi for first boot

In a shell move to folder `./deployments`

Make a copy of file `wpa_supplicant.conf.example` and rename it to `wpa_supplicant.conf`. Edit the file and replace the placeholders with your wifi`s SID and password.

Note: Select a 2.4 GHz wifi, otherwise the Raspi will not connect !

```shell
country=DE
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="<your network name>"
    scan_ssid=1
    psk="<your password>"
}
```

Save this file to the boot partition at `/Volumes/boot/wpa_supplicant.conf`

```shell

cp wpa_supplicant.conf /Volumes/boot/wpa_supplicant.conf

```


#### Step 3: Enable SSH

Put a file named **ssh** in the root of your Raspian`s boot partition.

```shell

touch /Volumes/boot/ssh

```


#### Step 4: Find your Raspberry Pi on the network

Eject the SD card from your computer, insert it into the Raspberry Pi and power it up. Wait for a couple of minutes until the Raspberry Pi has booted and started the ssh service.

```shell

ping raspberrypi.local

```


#### Step 5: Connect to your Raspberry Pi

In a shell use **ssh** to connect to your Raspberry Pi. Use the IP address returned by pinging the Pi.

```shell

ssh pi@<the_ip_address_of_the_pi>

```

The initial password is `raspberry`


#### Step 6: Change the password

Once logged in, change the password for the `pi` user

```shell

passwd

```


#### Step 7: Change the hostname

Change the hostname to something other than **raspberry** if you have more than one device.

```shell

sudo vi /etc/hostname

```


#### Step 8: Config the Raspberry Pi hardware and other OS settings

Use the configuration tool to change default settings.

```shell

sudo raspi-config

```

Set locales and timezone

* Localisation options | Change Locale
* Localisation options | Change Timezone

Enable HW interfaces

* Interfacing Options | ssh
* Interfacing Options | I2C
* Interfacing Options | Camera
* Interfacing Options | Remote GPIO

Expand the file system

* Advanced Options | Exapand Filesystem


Choose and hit enter. Reboot.


#### Step 9: Bring your Raspberry Pi up-to-date

Update your operating system to the latest version of everything.

```shell

sudo apt-get update

sudo apt-get upgrade

```


#### Step 10: Create a key-pair and install it

In order to login without a password, create a public/private key-pair and add the public key to the Raspberry Pi.

```shell

ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

```

Use `./edgepi` when being prompted for a key file name and location.

Finally, install you public key on the Raspberry Pi

```shell

cat ./edgepi.pub | ssh <user>@<hostname> 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'

```

You can now access the Raspberry Pi without a password:

```shell

ssh -i ./edgepi <user>@<hostname>

```

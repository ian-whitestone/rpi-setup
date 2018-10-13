# Setting Up a New Raspberry Pi

<img src='mmmmm-pie.jpg'>


1) I had an SD card pre-installed with NOOBS, so just plugged it in to power, a monitor and a keyboard, and was prompted to select my OS: `Raspbian [RECOMMENDED]`

- If you don't have an SD card with NOOBS, follow instructions [here](https://thepi.io/how-to-install-noobs-on-the-raspberry-pi/)
- If you are reformatting an SD card that used to have NOOBS/raspbian, I followed instructions [here](https://apple.stackexchange.com/questions/226016/how-to-remove-partition-on-sd-card-using-a-mac/269148)

2) Follow prompts
- Reset password
- Wifi setup

3) Enable SSH
- Instructions [here](https://www.raspberrypi.org/documentation/remote-access/ssh/)
- Get hostname by running `ifconfig`

4) SSH in..

```
→ ssh pi@192.168.0.22
The authenticity of host '192.168.0.22 (192.168.0.22)' can't be established.
ECDSA key fingerprint is SHA256:TSC2QT/GchXZT+vCVwBicj777+4g+qBwsi3iQn0HP5o.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.0.22' (ECDSA) to the list of known hosts.
pi@192.168.0.22's password:
Linux raspberrypi 4.14.71-v7+ #1145 SMP Fri Sep 21 15:38:35 BST 2018 armv7l

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sat Oct 13 10:24:49 2018
pi@raspberrypi:~ $ pwd
/home/pi
pi@raspberrypi:~ $ logout
Connection to 192.168.0.22 closed.
```

5) Renaming your PI

`$ sudo vi /etc/hostname`

6) Enable passwordless SSH

- Instructions [here](https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md)

7) Enable Camera

1) `$ sudo raspi-config`
2) Interfacing Options
3) Camera --> Yes


8) Static IP Address

Followed instructions [here](https://www.modmypi.com/blog/how-to-give-your-raspberry-pi-a-static-ip-address-update), and added the following to the end of my `/etc/dhcpcd.conf` file:

```
interface eth0

static ip_address=192.168.0.22/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1

interface wlan0

static ip_address=192.168.0.22/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1
```

9) Adding some SSH shortcuts
- Remember hostnames is annoying, so adding some ssh shortcuts to my personal macbook



10) Get oh-my-zsh

[Instructions](https://escapologybb.com/oh-my-zsh/)

```bash
sudo apt-get update && sudo apt-get upgrade 
sudo apt-get install git zsh
chsh -s /bin/zsh
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```


X) Install miniconda

`$ wget http://repo.continuum.io/miniconda/Miniconda3-latest-Linux-armv7l.sh`
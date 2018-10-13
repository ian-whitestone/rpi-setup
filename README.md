# Setting Up a New Raspberry Pi

<img src='mmmmm-pie.jpg'>


## Operating System
1) I had an SD card pre-installed with NOOBS, so just plugged it in to power, a monitor and a keyboard, and was prompted to select my OS: `Raspbian [RECOMMENDED]`

- If you don't have an SD card with NOOBS, follow instructions [here](https://thepi.io/how-to-install-noobs-on-the-raspberry-pi/)
- If you are reformatting an SD card that used to have NOOBS/raspbian, I followed instructions [here](https://apple.stackexchange.com/questions/226016/how-to-remove-partition-on-sd-card-using-a-mac/269148)

2) Follow prompts
- Reset password
- Wifi setup

### SSH Tings
1) Enable SSH
- Instructions [here](https://www.raspberrypi.org/documentation/remote-access/ssh/)
- Get hostname by running `ifconfig`

2) SSH in..

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

3) Renaming your PI

This is so I know which raspberry pi is which when I'm looking at connected devices on my router

`$ sudo vi /etc/hostname`

Whatever you change your hostname to, you need to add it to `/etc/hosts`.
For example, if you changed your hostname to `mypi`, add the following:

`127.0.0.1       mypi`

to the end of `/etc/hosts`

4) Enable passwordless SSH

- Instructions [here](https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md)

TLDR:

- If you already have a `~/.ssh/id_rsa.pub`, just do:

`cat ~/.ssh/id_rsa.pub | ssh pi@192.168.0.22 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'`

5) Static IP Address

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

6) Adding some SSH shortcuts
- Remember hostnames is annoying, so adding some ssh shortcuts to my personal macbook


`$ vi ~/.ssh/config`

```
Host pi1
    HostName 192.168.0.17
    User pi

Host pi2
    HostName 192.168.0.22
    User pi
```

### Camera Tings

1) Enable Camera

- `$ sudo raspi-config`
- Interfacing Options
- Camera --> Yes

2) OpenCV

3) Install some pythong packages

```
pip install picamera
pip install smbus2
```

4) pantilthat

- Enable I2C bus: 

    - `sudo raspi-config`
    - Interface Options
    - I2C

- Install drivers/packages: https://github.com/pimoroni/pantilt-hat

```bash
sudo apt-get install pimoroni
pip3 install pantilthat
```

### Other Tings

1) Get oh-my-zsh

[Instructions](https://escapologybb.com/oh-my-zsh/)

```bash
sudo apt-get update && sudo apt-get upgrade 
sudo apt-get install git zsh
chsh -s /bin/zsh

# Get oh-my-zsh
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```


### Conda

Install miniconda

```bash
wget http://repo.continuum.io/miniconda/Miniconda3-latest-Linux-armv7l.sh
sudo md5sum Miniconda3-latest-Linux-armv7l.sh
sudo /bin/bash Miniconda3-latest-Linux-armv7l.sh

# when prompted, change install path to /home/pi/miniconda3
```

Add the following to your `~/.bashrc` or `~/.zshrc`:

`export PATH=/home/pi/miniconda3/bin:$PATH`

Resource your profile:

`source ~/.zshrc`

And check your python path:

```bash
>>> which python
/home/pi/miniconda3/bin/python
```

Now install the anaconda-client so you can create conda environments:

```
sudo chown -R pi miniconda3
conda install anaconda-client
```

Create new environment:

`conda create -n test python=3 -y`

And activate it:

`source activate test`


Check which pip is the default:

`which pip`

If its not `/home/pi/miniconda3/bin/pip`, run a `conda install pip`, resource profile, and make sure `which pip` is now point to correct one.

`pip install --upgrade pip`


### Python 3

Conda python doesnt work as well on an RPI, particularly when trying to get the `pantilthat` library with I2C and smbus working.
Installing python 3 from source.


```
sudo apt-get install python3-dev libffi-dev libssl-dev -y
wget https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tar.xz
tar xJf Python-3.6.3.tar.xz
cd Python-3.6.3
./configure
make
sudo make install
sudo pip3 install --upgrade pip
```


### Security and System Monitoring

#### fail2ban
Coming soon...

#### Glances

Coming soon...
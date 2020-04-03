## Setup the Raspberry Pi development environment

In order to either develop or run software on the Raspberry Pi that uses its hardware interfaces, a number of packages and Python libraries are required.

### Install basic development dependencies

```shell

sudo apt-get install -y build-essential avahi-utils libopenjp2-7-dev libffi-dev libffi-dev libssl-dev \
  libtiff5-dev gfortran libatlas-base-dev libopenblas-dev libhdf5-serial-dev cdbs \
  git ntp sshpass

```

### Install Python3

```shell

sudo apt-get install -y python3 python3-dev python3-pip python3-virtualenv python3-numpy python3-pandas

```

### Install Raspberry hardware dependencies

```shell

sudo apt-get install -y i2c-tools python3-picamera python3-rpi.gpio

```

### Install Raspberry hardware add-on dependencies

```shell

sudo apt-get install -y joystick sense-hat

```

### Setup a python virtual env

```shell

python3 -m virtualenv -p python3 venv --system-site-packages
echo "source venv/bin/activate" >> ~/.bashrc
source ~/.bashrc

```


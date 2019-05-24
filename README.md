# rpi-tools
CLI utilities for working with Raspberry Pis.

## Commands
CLI commands executable from the `bin` directory.

### `exec_usb`
```
DESCRIPTION
    mount any available USB drive and execute any expected commands

USAGE
    ./exec_usb

OPTIONS
    -d, --directory
        path to directory containing media files (/mnt/usb)
```

### `multi_ssh`
```
DESCRIPTION
    execute shell commands across one or more remote hosts

USAGE
    ./multi_ssh --user pi --command "sudo reboot" --host 127.0.0.1 --host 127.0.0.2

OPTIONS
    -u, --user
        username to log in with

    -h, --host
        hostname or IP to access
    
    -c, --command
        command to execute on each remote client
```

### `pull_all`
```
DESCRIPTION
    execute `git pull` in all subdirectories of a given directory

USAGE
    ./pull_all -d /home/pi/src

OPTIONS
    -d, --dir
        git pull in all subdirectories of this path
```

### `set_ip`
```
DESCRIPTION
    overwrite network config to set static IP in /etc/dhcpcd.conf
    assumes netmask 255.255.255.0

USAGE
    ./set_static_ip --host 254 --subnet 1

OPTIONS
    -s, --subnet
        the subnet of the IP address (xxx.xxx.SUBNET.xxx)
    
    -h, --host
        the host part of the address (xxx.xxx.xxx.HOST)
```

### `set_wifi`
```
DESCRIPTION
    overwrite wifi config in /etc/wpa_supplicant/wpa_supplicant.conf

USAGE
    ./set_wifi --ssid NETWORK --password PASSWORD

OPTIONS
    -s, --ssid
        name of network to connect to
    
    -p, --password
        network password
```
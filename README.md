# rpi-tools
CLI utilities for working with Raspberry Pis.

## Recipies

### Automatically Run Git Pull For All Repos in /home/pi
```sh
sudo bin/runit \
    --name "autopull" \
    --command "/home/pi/rpi-tools/bin/pull_all --dir /home/pi" \
    --delay-seconds 30 \
    --user pi
```

### Automatically Mount Storage Volumes to /mnt/usb
```sh
sudo bin/runit \
    --name "automount" \
    --command "/home/pi/rpi-tools/bin/automount" \
    --exit-command "/home/pi/rpi-tools/bin/automount --unmount" \
    --delay-seconds 5
```

### Automatically Execute a File on a USB Drive Each Time It Is Mounted
```sh
sudo bin/runit \
    --name "autoexec" \
    --command "/home/pi/rpi-tools/bin/autoexec --path /mnt/usb/exec --daemon" \
    --delay-seconds 0
```

## Run A Python Process Forever
```sh
sudo bin/runit \
    --name "ledmatrix" \
    --command "PYTHONPATH=/home/pi/ledmatrix:$PYTHONPATH python3 /home/pi/ledmatrix/ledmatrix/animations/ticker.py --cols=42 --file /mnt/usb/ticker.txt" \
    --delay-seconds 0
```

## Run in "Kiosk Mode" Playing Media from Device
```sh
sudo bin/runit \
    --name "kiosk" \
    --command "/home/pi/rpi-tools/bin/kiosk --dir /mnt/usb --images --video" \
    --exit-command "/home/pi/rpi-tools/bin/kiosk" \
    --delay-seconds 0
```

### Automatically Reboot Every Hour
```sh
sudo bin/runit \
    --name "autoreboot" \
    --command "sleep 1h && sudo reboot"
```

## Commands
CLI commands executable from the `bin` directory.

### autoexec
```
DESCRIPTION
    call an executable each time it is created or modified

USAGE
    ./autoexec --path /mnt/usb/exec --daemon

OPTIONS
    -p, --path
        optional path to the executable (defaults to /mnt/usb/exec)

    -d, --daemon
        if set, runs forever in "daemon mode"
```

### automount
```
DESCRIPTION
    search /dev/sda* for any unmounted storage volumes and mount them to the given path

USAGE
    ./mount --path /mnt/usb --unmount

OPTIONS
    -p, --path
        optional path at which to mount the device (defaults to /mnt/usb)

    -u, --unmount
        optionally set this flag to *un*mount a volume mounted to the given path
```

### hotkeys
```
DESCRIPTION
    execute a command when a given key is pressed

USAGE
    ./hotkeys -a 'echo "you pressed a"' -b 'echo "you pressed b"'

OPTIONS
    -{char}
        a single-character flag followed by the command to run when this key is pressed
```

### install_common_deps
```
DESCRIPTION
    install common packages that may not be included in a fresh RPI

USAGE
    ./install_common_deps
```

### kiosk
```
DESCRIPTION
    kiosk mode supporting media playback from drive and network sources

USAGE
    ./kiosk --dir /mnt/usb --images --video --stream --port 8080 --udp-timeout 3 --img-timeout 4 --cleanup

OPTIONS
    -d, --dir
        path to directory containing media files (default is /mnt/usb)

    -i, --images
        set this flag to enable "slideshow mode" so the kiosk rotates through image files
        if paired with video or streaming, one random image will be shown between videos or streams

    -v, --video
        set this flag to enable "video mode" so the kiosk rotates through video files
        if paired with streaming or images, one random video will be shown between images or streams

    -s, --stream
        set this flag to enable "streaming mode" so the kiosk will listen for streamed media over UDP
        if paired with video or images, kiosk will fall back to other media when stream is interrupted

    -p, --port
        port to listen to for UDP-streamed media (default is 8080)

    --udp-timeout
        UDP TTL in seconds for streaming

    --img-timeout
        time to display images for in seconds
```

### multi_ssh
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

### pull_all
```
DESCRIPTION
    execute `git pull` in all subdirectories of a given directory

USAGE
    ./pull_all -d /home/pi/src

OPTIONS
    -d, --dir
        git pull in all subdirectories of this path
```

### runit
```
DESCRIPTION
    manage any executable with `runit` so it runs continuously in the background on boot

USAGE
    ./runit --name autoreboot --command "sudo reboot"  --delay-seconds 3600

OPTIONS
    -n, --name
        name of the runit-managed task

    -c, --command
        command or absolute path to executable (will be run as root)

    -d, --delay-seconds
        time to wait after the executable is called before running again
```

### set_ip
```
DESCRIPTION
    overwrite network config to set static IP in /etc/dhcpcd.conf
    assumes netmask 255.255.255.0

USAGE
    ./set_ip --host 254 --subnet 1

OPTIONS
    -s, --subnet
        the subnet of the IP address (xxx.xxx.SUBNET.xxx)

    -h, --host
        the host part of the address (xxx.xxx.xxx.HOST)
```

### set_wifi
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

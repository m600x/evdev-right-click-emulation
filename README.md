# evdev-right-click-emulation
## Planet Computers - Cosmo communicator [Linux V3]
---

*WIP:* Add support for right click on long press for the Cosmo Communicator Frankeinstein of a SmartLaptop by Planet Computers.

Current state:
- Working mostly
- Cannot right click twice, you must click somewhere else before long press again
- Cannot right click on taskbar

## Build
```
sudo apt install build-essential libevdev-dev git -y
git clone https://github.com/m600x/cosmo-rightclick.git
cd cosmo-rightclick
make all
```

## Run
```
out/cosmo-rightclick
```

## Install as service
```
sudo cp out/cosmo-rightclick /usr/local/bin/cosmo-rightclick
sudo vi /etc/systemd/system/rightclick.service
```

Content of `/etc/systemd/system/rightclick.service`:
```
[Service]
Type=oneshot
RemainAfterExit=true
StandardOutput=journal
ExecStart=/usr/local/bin/cosmo-rightclick
[Install]
WantedBy=default.target
```
Reboot using `sudo reboot now`

---

Partial original readme below

---

Note: this program needs to read from and write to `/dev/input`. Make sure the current user has the permission to do so, or you may use `root`.

There are some tunable options accessible through the following environment variables:

```bash
# Required interval in ms for a touch event to be considered a long press
# default = 1000ms
LONG_CLICK_INTERVAL=1000
# Allowed margin of error (in px) for a long press event
# i.e. the finger is considered still if its movement is within this range
# Note: the px here is not physical pixels on the screen. It's the physical
# pixels of the touch devices, which may have a different resolution.
# default = 100px
LONG_CLICK_FUZZ=100
# Uncomment to set a blacklist of devices that `evdev-rce` will not
# listen on for touch events. The names can be retrived by using `xinput`
# or simply by reading the output of `evdev-rce`
# (it will print out the names of all touch devices when starting,
#  e.g. "Found touch screen at <some_path>: <device_name>)
#TOUCH_DEVICE_BLACKLIST="device1 name|device2 name|..."
# Uncomment to set a whitelist of devices that `evdev-rce` will ONLY
# listen on. This overrides the blacklist - when whitelist is present,
# any device not in this list will be ignored.
#TOUCH_DEVICE_WHITELIST="device1 name|device2 name|..."
```

So you can run the program like

```
LONG_CLICK_INTERVAL=500 LONG_CLICK_FUZZ=50 cosmo-rightclick
```

To customize those options.

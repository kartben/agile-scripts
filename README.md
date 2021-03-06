# AGILE scripts

This repository contains start and stop scripts for various [AGILE](http://agile-iot.eu/) components.
Currently, AGILE is tested on Raspberry PI 2/3 Model B running Raspbian, and on x86_64 running Ubuntu 16.04.
Since we use docker containers, it should also run in many other environment.

## Requirements

### On Raspberry PI 2/3 Model B running Raspbian

- Install recent version of Raspbian

Download image from [Raspbian](https://downloads.raspberrypi.org/raspbian/images/raspbian-2016-05-31/2016-05-27-raspbian-jessie.zip)

Write it to SD, using
```
sudo dd bs=1m if=2016-05-27-raspbian-jessie.img of=/dev/<YOUR_uSD_DISK>
```

- Start the Pi and log in
```
ssh pi@raspberrypi.local
```

Default password is "raspberry"

- Install recent versions of `docker` and `docker-compose`

Install docker from [Hypriot](http://blog.hypriot.com/post/your-number-one-source-for-docker-on-arm/)

```
curl -s https://packagecloud.io/install/repositories/Hypriot/Schatzkiste/script.deb.sh | sudo bash
sudo apt-get install -y docker-hypriot docker-compose
sudo usermod -a -G docker pi
# you can make the group change effective without logout/login (http://superuser.com/questions/272061/reload-a-linux-users-group-assignments-without-logging-out)
sudo su - $USER
```

- Disable and stop Bluez in the host

Unfortunately Bluez on the Raspbian is too old. Our BLE driver needs version 5.39 with experimental features enabled. Bluez is part of the agile-core container, thus it should be disabled in the host OS.

```
sudo systemctl disable bluetooth
sudo systemctl stop bluetooth
```

### On other machines (x86_64)

On other machines, install docker (at least version 1.11) and docker-compose (at least version 1.8). Check it with `docker -v` and `docker-compose -v`.
Read more on how to [install or upgrade](https://docs.docker.com/compose/install/)

- Disable and stop Bluez in the host

```
sudo systemctl disable bluetooth
sudo systemctl stop bluetooth
```

## Getting AGILE

- Pull our library to have AGILE startup scripts, if not yet done.
```
git clone https://github.com/Agile-IoT/agile-scripts.git
cd agile-scripts
```

## Usage

Use `./agile start` to start the main components, and `./agile stop` to stop them.

Once component started you can visit http://127.0.0.1:8000 to access the AGILE user interface and start building your IoT solution.

To access the gateway from LAN/WLAN, http://raspberrypi.local:8000 might work, depending on your network setup. If not, go by IP.

## Update

Use `./agile update` to download the newest version of AGILE component. Note that this is different from `git pull`, which updates the
startup scripts only.


### Updating a single component

In most cases, you can update a single coponent while other components keep running.
For example, to update agile-osjs without restarting the rest, use the following commands:
```
compose/agile-compose stop agile-osjs
compose/agile-compose pull agile-osjs
compose/agile-compose up agile-osjs
```

## Troubleshooting

You can access the logs with `./compose/agile-compose logs` and view all logs.

To view per component log use `./compose/agile-compose logs <component>`

To get the list of running components use `./compose/agile-compose ps` with the pattern `compose_<component name>_1`

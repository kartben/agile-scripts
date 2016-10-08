AGILE scripts
===

start and stop scripts for various AGILE components

Requirements
---

- Recent versions of `docker` and `docker-compose`

On a Raspberry Pi, install docker from [Hypriot](http://blog.hypriot.com/post/your-number-one-source-for-docker-on-arm/)

```
curl -s https://packagecloud.io/install/repositories/Hypriot/Schatzkiste/script.deb.sh | sudo bash
sudo apt-get install -y docker-hypriot docker-compose
sudo usermod -a -G docker pi
```

On other machines, install at least version 1.8. Check it with `docker-compose -v`.
Read more on how to [install or upgrade](https://docs.docker.com/compose/install/)

- You can make the [group change effective without logout/login](http://superuser.com/questions/272061/reload-a-linux-users-group-assignments-without-logging-out)
```
`su - $USER`
```

- Disable Bluez in the host

Unfortunately the Raspbian Bluez is too old. Our BLE driver needs version 5.39 with experimental features enabled. Bluez is part of the agile-core
container, thus it should be disabled in the host OS.

```
sudo systemctl disable bluetooth
```

Getting AGILE
---

- Pull our library to have AGILE startup scripts, if not yet done.
```
git clone https://github.com/Agile-IoT/agile-scripts.git
cd agile-scripts
```

Usage
---

Use `./agile start` to start the main components, and `./agile stop` to stop them.

Once component started you can visit http://127.0.0.1:8000 to access the AGILE user interface and start building your IoT solution.

To access the gateway from LAN/WLAN, http://raspberrypi.local:8000 might work, depending on your network setup. If not, go by IP.

Update
---

Use `./agile update` to download the newest version of AGILE component. Note that this is different from `git pull`, which updates the
startup scripts only.


Updating a single component
---

In most cases, you can update a single coponent while other components keep running.
For example, to update agile-osjs without restarting the rest, use the following commands:
```
compose/agile-compose stop agile-osjs
compose/agile-compose pull agile-osjs
compose/agile-compose up agile-osjs
```

Troubleshooting
---
You can access the logs with `./compose/agile-compose logs` and view all logs.

To view per component log use `./compose/agile-compose logs <component>`

To get the list of running components use `./compose/agile-compose ps` with the pattern `compose_<component name>_1`

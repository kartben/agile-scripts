
Starting AGILE components with Docker
===

This folder contains scripts to start AGILE components as Docker containers

Prerequisites
---
- install docker from [Hypriot](http://blog.hypriot.com/post/your-number-one-source-for-docker-on-arm/)
```
curl -s https://packagecloud.io/install/repositories/Hypriot/Schatzkiste/script.deb.sh | sudo bash
sudo apt-get install -y docker-hypriot docker-compose
sudo usermod -a -G docker pi
```
- you can make the [group change effective without logout/login](http://superuser.com/questions/272061/reload-a-linux-users-group-assignments-without-logging-out)
```
`su - $USER`
```

- update Bluez for up-to-date BLE support

Unfortunately the Raspbian Bluez is too old. You should update to version 5.39. Draft instructions follow. Note that this will be simplified in the near future:

```
sudo apt-get install libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.39.tar.xz
tar xf bluez-5.39.tar.xz
cd bluez-5.39
./configure
make -j 4
sudo make install
```

Enable experimental mode by changing the following line in /lib/systemd/system/bluetooth.service
```
ExecStart=/usr/local/libexec/bluetooth/bluetoothd --experimental
```

Update /etc/init.d/bluetooth to point to the just installed version in /usr/local
```
24c24,25
< PATH=/sbin:/bin:/usr/sbin:/usr/bin
---
> PATH=/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
27,28c28,31
< DAEMON=/usr/sbin/bluetoothd
< HCIATTACH=/usr/bin/hciattach
---
> DAEMON=/usr/local/libexec/bluetooth/bluetoothd
> HCIATTACH=/usr/local/bin/hciattach
33c36,37
< SDPTOOL=/usr/bin/sdptool
---
> SDPTOOL=/usr/local/bin/sdptool
```

Then, restart bluez:
```
sudo systemctl daemon-reload
sudo service bluetooth restart
```

Getting AGILE
---

- pull our library to have docker-scripts
```
mkdir -p AGILE && cd AGILE
git clone https://github.com/Agile-IoT/agile-scripts.git
cd agile-scripts
```

- pull ARM image file and start AGILE
```
docker pull agileiot/agile-core-armv7l:latest # Not strictly needed, as the container is also pulled by agile-core-start, if not done before.
```

Starting/Stopping AGILE
---

- starting AGILE

```
docker-scripts/agile-core-start
. docker-scripts/agile-env # Expose AGILE D-Bus in shell. 
```

- stopping AGILE
```
sudo docker-scripts/agile-stop
```

- alternative start AGILE with per-service containers
```
docker-scripts/agile-core-perservice-start
```

Testing the installation
---

```
sudo apt-get install python3-dbus -y # Make sure D-Bus python support is available.
. docker-scripts/agile-env # Set up environment to connect to AGILE D-Bus.                                                      
test/discover.py
```

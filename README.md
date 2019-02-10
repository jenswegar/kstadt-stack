# kstadt stack

This is a docker stack I use for collecting RuuviTag metrics and storing them in a local InfluxDB, which in turn is used by Grafana to graph the metrics on a dashboard.

## Hardware

- Raspberry Pi 2 Model B

## Software
- Raspbian Stretch Lite
- 

### Additional software

Git
```
sudo apt-get update && sudo apt-get install git
```

Clone this repository to a folder on the Rpi


Install Docker
```
curl -sSL https://get.docker.com | sh
```

Add pi user to docker group to run docker without sudo

```
sudo usermod -aG docker pi
```

Install docker-compose:
```
sudo apt install -y python python-pip
pip install docker-compose
```

Add .local/bin to path so docker-compose can be executed anywhere
```
vi ~/.bashrc
```

Add the following at end of .bashrc
```
export PATH=$HOME/.local/bin:$PATH
```
exit and save, then
```
source ~/.bashrc
```
to have changes take effect


### Setup RuuviCollector for collecting metrics

clone https://github.com/Scrin/RuuviCollector/ to the Rpi

```
git clone https://github.com/Scrin/RuuviCollector.git
```

and build docker image locally with

```
docker build -t ruuvi-collector .
```

other requirements (see RuuviCollector setup instructions): 
```
sudo apt-get install bluez-hcidump
```

```
sudo setcap 'cap_net_raw,cap_net_admin+eip' `which hcitool`
sudo setcap 'cap_net_raw,cap_net_admin+eip' `which hcidump`
```


## Running the stack

cd into the kdstadt-stack project folder. 
```
cp env.dist .env
```
Then fill your own values in the ```.env``` file

In the same folder, create the files
- ruuvi-collector.properties
- ruuvi-names.properties

based on the example files and fill them out with your own values. (see RuuviCollector project for help on these files). One suggestion is to set the whitelist filter to allow only the mac-addresses of the tags.

Run the stack
```
docker-compose up -d
```


## Troubleshooting

It seems the stack stops collecting metrics sometimes, for no apparent reason (still need to investigate this).

The first thing to try would be a clean reboot, by ssh into the rpi and issuing the command

```
sudo reboot now
```

Usually this helps, but if not, some more commands to try in order to reset the hci and bluetooth daemons.

```
sudo hciconfig hci0 down
sudo hciconfig hci0 up

sudo service bluetooth restart
sudo service dbus restart
```


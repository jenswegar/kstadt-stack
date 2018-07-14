Followed steps to setup docker on Rpi with Raspbian Stretch Lite:

Install docker
curl -sSL https://get.docker.com | sh


Install docker-compose:

# Install required packages
apt update
apt install -y python python-pip

# Install Docker Compose from pip
pip install docker-compose


vi ~/.bashrc

export PATH=$HOME/.local/bin:$PATH

exit and save

then
source ~/.bashrc

to have changes take effect


clone https://github.com/Scrin/RuuviCollector/ and build docker image locally with

docker build -t ruuvi-collector .



Run the stack

docker-compose up -d


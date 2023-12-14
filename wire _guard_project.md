# Wire Guard Project

## Part 1 (Creating a Digital Ocean Account)

Use this exact url to create a new Digital Ocean account: https://m.do.co/c/4d7f4ff9cfe4

- You can also got onto Digital Ocean by searching it up on Google normally, but using this link gets you $200 worth of credit for two months, so, in my opinion, using the link listed is a better choice than creating your own Digital Ocean account normally.

- If you already do ave a Digital Ocean account, either create a new account using a different email address with the link, or delete the one you already made if you do not actively use it.

## Part 2 (Create a Droplet)

First of all, choose Ubuntu as your OS, set the version to match whatever version of Ubuntu you have installed on your VM, (I would recommend the 20.04 (LTS) x64 version of Ubuntu) and make sure the CPU is set to "Regular."

Secondly, choose what kind of droplet you want to make.

- I would recommend the cheapest option possible. Although, if you signed up with the link, it probably won't matter what kind of droplet you make, considering you will have $200 worth of credit, which should be more than enough credit to do what you are about to do.

Thirdly, choose to use either an SSH key or a password.

- I would personally go with the password option here, but the SSH option IS more secure, so if you prefer security over simplicity, feel free to go with the SSH option.

Fourth, choose what Datacenter you want to have your droplet in for the region you specified.

Finally, you don't really need to customize your droplet anymore than you have already through the steps I have specified, so feel free to go to the next step.

## Part 3 (Installing Docker on your Droplet VM)

Before you do anything, make sure you check for anything that can be upgraded in your terminal.

- You can do this by typing the following command in your Droplet terminal: `sudo apt update` followed by `sudo apt upgrade`

After you have checked for upgrades in your VM, type the following command: `sudo apt install docker.io`

Next, you need to install Pip. Do this with the following command: `sudo apt install pip`

Check for updates again, and if you see any, upgrade.

Run the following command to install docker-compose: `sudo pip3 install docker-compose`

## Part 4 (Installing Wireguard On Your Droplet VM with Docker)

Type the following commands in your terminal:
`mkdir -p ~/wireguard`
`mkdir -p ~/wireguard/config`
`nano ~/wireguard/docker-compose.yml`

In the newly created `/wiregaurd/docker-compose.yml` file, add in the following credentials:
"version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Hong_Kong
      - SERVERURL=1.2.3.4
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1"

- It should be noted that, in this file, you are going to change a few things.

  - Change the timezone. This is noted by the "TZ=" credential in the environment section.
  - Change the SERVERURL credential to read as whatever the IP address of your droplet is.
  - Save the file after you are done.

Run this command after you are done editing the aforementioned file: `sudo apt install apt-transport-https ca-certificates curl software-properties-common -y`

After that command, type this one: `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

- This command should add a key.

- The afforementioned command will depend the correct repository. For me, because I was using a 32 bit/64 bit OS, that one was the one that worked for me.

Type this command: `sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"`

Type this command in: `apt-cache policy docker-ce`

Type following command in as well: `sudo apt install docker-ce -y`

Run the following command: `sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

Start Wireguard by running the following commands:
`cd ~/wireguard`
`sudo docker-compose up -d`

## Part 5 (Testing Your VPN On Your Mobile Device)

Install the Wireguard App on your Mobile Device.

Run the following command in your Ubuntu Terminal:
`sudo docker-compose logs -f wireguard`

- It should create a QRCode in your terminal.

Open up the Wireguard app on your phone, click the plus sign, and tap on the option that says "Create from QR code."

Click on the newly create tunnel to view its status

- If it says its active, then that means you got your VPN running up and working!

## Part 6 (Testing Your VPN On Your PC)

I have to honest, I was not able to figure out how to get this working. Sorry.

# Screenshots

IP Address: [!RegIP](https://github.com/TeeSocks2/Wireguard-Project/issues/1)

PEER phone1: [!Peer](https://github.com/TeeSocks2/Wireguard-Project/issues/2)

VM IP Address: [!VmIP](https://github.com/TeeSocks2/Wireguard-Project/issues/3)

# Wireguard Project Documentation
## Philip Rahal
## 11 December 2024

# Step 1: Create Digital Ocean Account
Follow this link to create an account and receive $200 credit for 2 months of usage.
https://m.do.co/c/d33d59113ab6

# Step 2: Create Droplet
Select the "Create" drop-down menu in the top right corner, and choose "Droplets"
Select the following:
1. Ubuntu 24.04
2. Basic
3. Regular Intel CPU
4. Normal SSD
5. Choose SSH Key or Password (I chose Password)
6. Leave default data center.

# Step 3: Install Docker and Wireguard
Install Docker using this command:
sudo apt install docker

Setup Wireguard:
Run these commands on the Droplet:
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
nano ~/wireguard/docker-compose.yml

Copy+Paste the following:
version: '3.8'
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
      - net.ipv4.conf.all.src_valid_mark=1

Manually change TZ (timezone), SERVERURL (Droplet IP), and PEERS (select how many peers you want).

Start Wireguard:
cd ~/wireguard/
docker-compose up -d

# Step 4: Connect to Wireguard on Mobile Device
1. Install Wireguard from AppStore or GooglePlay
2. Click the '+' icon in the bottom right
3. Select "Create from QR Code"

# Step 5: Connect to Wireguard on Laptop/PC
1. Install Wireguard on local environment
2. Copy config file from Droplet (I did this using WinSCP)
3. Enter Droplet's IP, Username and Password
4. Connect

# Step 6: Proof of Connection
Use IPLeak.net to show connection status before and after connecting to Wireguard (on Mobile Device and on Laptop/PC).

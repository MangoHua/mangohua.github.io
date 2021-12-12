# VPN
# Install Docker
Frist install Docker on Ubuntu20.04:
         
	 sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
         
	 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
         
	 sudo add-apt-repository \
            "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
            $(lsb_release -cs) \
            stable"
if it shows you have 1 not upgraded, you could use the commond: 
	 
	 sudo apt-get upgrade
For the Next step
	 
	 apt-cache policy docker-ce
	 
	 sudo apt install docker-ce -y
	 
	 Then we try to install Docker-compose:
	 
	 sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
	 
	 sudo chmod +x /usr/local/bin/docker-compose
# Wireguard	 
Then we set up Wireguard
	
	mkdir -p ~/wireguard/
        
	mkdir -p ~/wireguard/config/
        
	nano ~/wireguard/docker-compose.yml
Next, the content we could put into docker-compose.yml                               
         
	 version: '3.8'
         services:
           wireguard:
             container_name: wireguard
             image: linuxserver/wireguard
             environment:
               - PUID=1000
               - PGID=1000
               - TZ=America/Chicago
               - SERVERURL=143.110.152.242
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
TZ: you change to the TZ database time zones

Serverurl: put server IP address

Now we can start Wireguard
         
	 cd ~/wireguard/
         
	 docker-compose up -d
When it say wireguard is up-to-date, so we could do the next
Then use this command to form the QR code
        
	docker-compose logs -f wireguard
On your phone, after download Wireguard, scan the QR code

Go to ipleak.net to Check it is connect successful or not.

Next step: Connect on the Laptop

Use command ls, to list the file

Use command cd, to get into the file called wireguard

Agian ls to list the file in wireguard

cd to get in to the file called config
         
	 ls
         
	 cd wireguard
         
	 ls
         
	 cd config
         
	 ls
         
	 nano peer_pc1
you can see the content in the peer_pc1, and copy content inside the peer_pc1 and paste to the Notepad and save it in .conf

Then 

open the file in the Wireguard

Turn it on

Go to ipleak.net to check connect

So you are done!

#Pihole download steps

Website followed: https://github.com/pi-hole/docker-pi-hole

1. Install docker #Steps aren't shown

2. nano docker-compose.yml #Craete yml file for pihole


3. Put this block of text into docker-compose.yml

version: "3"

# More info at https://github.com/pi-hole/docker-pi-hole/ and https://docs.pi-hole.net/
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    # For DHCP it is recommended to remove these ports and instead add: network_mode: "host"
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp" # Only required if you are using Pi-hole as your DHCP server
      - "80:80/tcp"
    environment:
      TZ: 'America/Chicago'
      # WEBPASSWORD: 'set a secure password here or it will be random' #This line was uncommented and had proper password set.
    # Volumes store your data between container upgrades
    volumes:
      - './etc-pihole:/etc/pihole'
      - './etc-dnsmasq.d:/etc/dnsmasq.d'
    #   https://github.com/pi-hole/docker-pi-hole#note-on-capabilities
    cap_add:
      - NET_ADMIN # Required if you are using Pi-hole as your DHCP server, else not needed
    restart: unless-stopped

4. sudo docker compose up -d #Build and start docker
a. Find IP address: sudo docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' pihole

5. Go to firefox on VM and go here:
http://IPaddress/admin

Optional step (when restarting Pihole). 
sudo systemctl stop systemd-resolved #Clear port when needed

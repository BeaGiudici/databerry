#dockerimage

Write the following `docker-compose.yaml`:

```
# More info at https://github.com/pi-hole/docker-pi-hole/ and https://docs.pi-hole.net/
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      # DNS Ports
      - "53:53/tcp"
      - "53:53/udp"
      # Default HTTP Port
      - "80:80/tcp"
      # Default HTTPs Port. FTL will generate a self-signed certificate
      - "443:443/tcp"
      # Uncomment the line below if you are using Pi-hole as your DHCP server
      #- "67:67/udp"
      # Uncomment the line below if you are using Pi-hole as your NTP server
      #- "123:123/udp"
    environment:
      # Set the appropriate timezone for your location (https://en.wikipedia.org/wiki/List_of_tz_database_time_zones), e.g:
      TZ: 'Europe/London'
      # Set a password to access the web interface. Not setting one will result in a random password being assigned
      FTLCONF_webserver_api_password: '<SET-YOUR-PASSWORD-HERE>'
      # If using Docker's default `bridge` network setting the dns listening mode should be set to 'all'
      FTLCONF_dns_listeningMode: 'all'
    # Volumes store your data between container upgrades
    volumes:
      # For persisting Pi-hole's databases and common configuration file
      - './etc-pihole:/etc/pihole'
      # Uncomment the below if you have custom dnsmasq config files that you want to persist. Not needed for most starting fresh with Pi-hole v6. If you're upgrading from v5 you and have used this directory before, you should keep it enabled for the first v6 container start to allow for a complete migration. It can be removed afterwards. Needs environment variable FTLCONF_misc_etc_dnsmasq_d: 'true'
      #- './etc-dnsmasq.d:/etc/dnsmasq.d'
    cap_add:
      # See https://github.com/pi-hole/docker-pi-hole#note-on-capabilities
      # Required if you are using Pi-hole as your DHCP server, else not needed
      - NET_ADMIN
      # Required if you are using Pi-hole as your NTP client to be able to set the host's system time
      - SYS_TIME
      # Optional, if Pi-hole should get some more processing time
      - SYS_NICE
    restart: unless-stopped
```

Be careful that other services might be using the port you are setting. In particular, I had problems with:
- `port 53`: disable `systemd-resolved`:
	1. `sudo systemctl stop systemd-resolved`
	2. `sudo systemctl stop systemd-resolved`
	3. Modify `/etc/resolv.conf` putting `nameserver 1.1.1.1`
- `port 80`: edit the `docker-compose.yaml` file from `80:80` to `8080:80`.

When everything is set up, you can connect to the web UI with `http://<your-pi-ip>:8080/admin`, using the password you set in the image, at `FTLCONF_webserver_api_password` (line 22 of the image above).

## Web Interface
You can connect to the web interface using http://<ip-address>:<port-number>. This will allow you to change the options of pi-hole in a nice GUI environment.

It is useful to set up a DNS, so that we can connect to databerry using a hostname, without memorizing its IP address. In order to do so, go to **Settings** -> **Local DNS Records**. Here, you can add the domain and its IP address. Usually, most systems (especially systemd-resolved) are fussy about "bare" hostnames unless they are defined in `/etc/hosts` or the system is set to search domain suffixes (like `.local`, `.lan`, etc.). I am not a fan of changing `/etc/hosts`, so I opted for setting a suffix. You can set a common domain name in Pi-hole, just go under **Settings** -> **DNS** -> **DNS domain settings**. Here, you can set a Pi-hole domain name. Flag the "Expand hostnames" option to automatically expand the hostnames. 
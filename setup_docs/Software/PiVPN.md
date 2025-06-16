#software

## Installation and setup
1. Download and start the installation:
	```
	curl -L https://install.pivpn.io | bash
	```

2. Follow the guided installation and choose [[WireGuard]]. 
3. During the process, it will let you select a static IP. I choose the one set by the router.
4. Forward VPN port on the router:
	- Default port: 51820
5. When asked for DNS provider, choose `Custom` and enter the [[pihole]] IP.
6. Create a profile with `pivpn add -n <profile-name>`

## Usage
In the following, let's see how to use the VPN.

### Mobile
Download the [WireGuard App](https://play.google.com/store/apps/details?id=com.wireguard.android), and scan the QR code obtained with `pivpn -qr`.

### Laptop
Install WireGuard on your laptop: `sudo apt install wireguard`. Once that's done, copy the config file generated on the server to `/etc/wireguard/`.

You can connect to the vpn using:
```
sudo wg-quick up <myprofile>
```
Once you run this command you are securely connected to your home network over VPN. 
This means that you can ssh to databerry!

You can disconnect with:
```
sudo wg-quick down <myprofile>
```

**Pro-tip**: If you want to make VPN connection automatic at boot or login add `wg-quick up <myprofile>` to your `.bashrc`, `.profile`, or a systemd service.
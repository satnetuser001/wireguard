# WireGuard VPN server in Docker container
Custom image based on Ubuntu for quick deployment.  
Note: to speed up the transfer of the VPN server to another host, you can save the `wg0.conf` file after completing the setup.

### Server system requirements:
- public IP address 
- docker
- absence of docker images, containers, or networks named `wireguard`
- unoccupied port `51820` (you can use another port number)

### Server setup
1. Clone this repository.
```bash
git clone https://github.com/satnetuser001/wireguard.git
````
3. Navigate to the `wireguard` directory.
```bash
cd wireguard
```
3. Optionally. Change the WireGuard port number to a non-standard.  
In the `compose.yml` file, under the `ports` parameter, change the `published` value to your own in the range 1–65535.
4. Generate key files for the server and all clients.  
Note: key files can be generated on the client side for convenience.
```bash
wg genkey | tee server_private.key | wg pubkey > server_public.key
```
```bash
wg genkey | tee client_1_private.key | wg pubkey > client_1_public.key
```
```bash
wg genkey | tee client_2_private.key | wg pubkey > client_2_public.key
```
```bash
wg genkey | tee client_N_private.key | wg pubkey > client_N_public.key
```
5. Add the contents of the generated keys to the `wg0.conf` file.  
Note: the `wg0.conf` file contains hints where to insert each key.  
Important: add the necessary number of `### CLIENTs ###` entries or remove the excess ones in the `wg0.conf` file.
6. Up the container.
```bash
docker compose up -d
```
7. If you need to down the container.
```bash
docker compose down --timeout 1
```
8. If you need to rebuild the image and restart the container to apply new settings (e.g., port).
```bash
docker compose up -d --build --force-recreate
```  

### Client setup
Note: the example below is for Ubuntu, documentation for other operating systems can be found on the official [WireGuard](https://www.wireguard.com/quickstart/) website. WireGuard applications are also available for mobile devices.
1. Install WireGuard.
```bash
sudo apt install wireguard -y
```
2. Open the `wg0.conf` configuration file.
```bash
sudo nano /etc/wireguard/wg0.conf
```
or if you have Sublime Text
```bash
sudo subl /etc/wireguard/wg0.conf
```
3. Add the contents of the generated keys to the `wg0.conf` file.  
Note: use the example below.  
Important: standard value for `SERVER_PORT_HERE` is `51820`, if you didn't change it in `Server setup` step 3.
```text
### CLIENT_1 ###
[Interface]
PrivateKey = CLIENT_1_PRIVATE_KEY_HERE
Address = 10.0.0.2/24
DNS = 8.8.8.8

### SERVER ###
[Peer]
PublicKey = SERVER_PUBLIC_KEY_HERE
Endpoint = SERVER_PUBLIC_IP_HERE:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```
```text
### CLIENT_N ###
[Interface]
PrivateKey = CLIENT_N_PRIVATE_KEY_HERE
Address = 10.0.0.N+1/24
DNS = 8.8.8.8

### SERVER ###
[Peer]
PublicKey = SERVER_PUBLIC_KEY_HERE
Endpoint = SERVER_PUBLIC_IP_HERE:SERVER_PORT_HERE
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```
4. Up WireGuard connection.
```bash
sudo wg-quick up wg0
```
4. Down WireGuard connection.
```bash
sudo wg-quick down wg0
```
5. Enable automatic connection up on system boot.
```bash
sudo systemctl enable wg-quick@wg0
```
6. Disable automatic connection up on system boot.
```bash
sudo systemctl disable wg-quick@wg0
```
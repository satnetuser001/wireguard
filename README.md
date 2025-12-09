# WireGuard VPN server running in a Docker container
Self-made image based on Ubuntu

### System requirements:
- docker
- absence of docker images, containers, or networks named wireguard  
- unoccupied port 51820 (or the port number you selected)

### Server configuration
1. Clone this repository.
```bash
git clone https://github.com/satnetuser001/wireguard-container.git
````
3. Navigate to the `wireguard` directory.
```bash
cd wireguard
```
3. Optionally. Change the WireGuard port number to a non-standard.
In the `compose.yml` file, under the `ports` parameter, change the `published` value to your own.
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
Note: the wg0.conf file contains hints where to insert each key.
Important: Add the necessary number of `### CLIENTs ###` entries or remove the excess ones in the `wg0.conf` file.
6. Up the container.
```bash
docker compose up -d
```
7. If it is necessary down the container.
```bash
docker compose down -t 1
```
# wireguard-install

Set up a self-hosted WireGuard VPN server for clients that connect from outside your network. The shell script walks you through server configuration and creates a client configuration file.

## Features

- Configure a WireGuard server through an interactive setup assistant.
- Add more users by running the installer again.
- Set the private subnet, public server address, listening port, and client DNS through environment variables.
- Run without prompts by setting `INTERACTIVE=no`.

## Installation

The original installer targets Ubuntu 18.04 LTS, Debian 9, and CentOS 7. **These are legacy targets; support for current distributions is not established.**

Download or clone this project's source, then open a shell in its directory:

```sh
cd wireguard-install
```

Review `wireguard-install.sh` before running it. The installer requires root privileges to configure the server.

## Usage

Start the interactive assistant:

```sh
sudo bash wireguard-install.sh
```

To choose a listening port and client DNS server while keeping the interactive prompts:

```sh
sudo env SERVER_PORT=51820 CLIENT_DNS=1.1.1.1 bash wireguard-install.sh
```

The installer accepts these environment variables:

| Variable | Purpose | Default |
| --- | --- | --- |
| `INTERACTIVE` | Set to `no` to disable prompts | Interactive prompts enabled |
| `PRIVATE_SUBNET` | Private VPN subnet | `10.9.0.0/24` |
| `SERVER_HOST` | Public server IP address | Automatically detected |
| `SERVER_PORT` | WireGuard listening port | Randomly selected |
| `CLIENT_DNS` | Comma-separated DNS servers for clients | Not specified here |

Run the script again to add more users. Reboot the server after installation to apply all settings.

### Connect a Linux client

Install WireGuard on the client using your distribution's package manager. Copy `/root/client-wg0.conf` from the server to `/etc/wireguard/wg0.conf` on the client.

**Treat the client configuration as a secret: it contains a private key.**

On a systemd-based client with `wg-quick` installed, start the connection and inspect its status:

```sh
sudo systemctl start wg-quick@wg0.service
sudo wg show
```

## License

MIT. This project is based on [l-n-s/wireguard-install](https://github.com/l-n-s/wireguard-install), which is licensed under MIT. Retain the original copyright and license notices when redistributing its code or substantial portions of it.

# MosDNS v5 Configuration

This repository contains configuration files for [MosDNS](https://github.com/IrineSistiana/mosdns) version 5, designed to intelligently handle DNS requests using rule-based routing.

## Overview

MosDNS is a flexible DNS forwarder that supports custom forwarding rules. This configuration implements the following features:

- **Intelligent Domain Routing**: Routes DNS queries to appropriate upstream servers based on domain lists
- **GFW List Support**: Routes domains blocked by the Great Firewall to foreign DNS servers
- **China Domain List**: Routes Chinese domains to local DNS servers for optimal speed
- **Ad Blocking**: Blocks DNS queries for advertising domains
- **EDNS Client Subnet (ECS)**: Improves CDN routing by specifying client location
- **DNS-over-TLS Support**: Encrypts DNS queries for enhanced privacy

## Directory Structure

- `config.yaml`: Main configuration file for MosDNS
- `*.txt` files: Domain and IP lists for different routing rules
- `update`: Script to update domain and IP lists from online sources
- `hosts`: Custom host mappings

## Domain and IP Lists

This configuration uses several domain and IP lists:

- `china-list.txt`: Chinese domains that should use local DNS
- `direct-list.txt`: Domains that should always use direct access
- `gfw.txt`: Domains blocked by GFW that require proxy DNS
- `proxy-list.txt`: Domains that should use proxy DNS
- `reject-list.txt`: Domains that should be blocked (ad domains, etc.)
- `cn.txt`: China IP ranges
- `CDN_not_follow_ecs.txt`: CDN IPs that don't follow EDNS Client Subnet
- `lan.txt`: Local network IP ranges

## Installation

1. Install MosDNS v5 from the [official repository](https://github.com/IrineSistiana/mosdns)
2. Clone this repository:
   ```
   git clone https://github.com/jianxx/mosdns-v5-config.git
   cd mosdns-v5-config
   ```
3. Update the domain and IP lists:
   ```
   chmod +x update
   ./update
   ```
4. Adjust the configuration in `config.yaml` if needed:
   - Change the listening port (default: 5335)
   - Update the ECS IP address to your local ISP
   - Modify upstream DNS servers if needed
   - Adjust SOCKS5 proxy settings for foreign DNS servers

## Usage

1. Start MosDNS with the configuration:

   ```
   mosdns start -d /path/to/mosdns-v5-config -c config.yaml
   ```

2. Configure your devices to use MosDNS as the DNS server (default port: 5335)

3. Periodically update the domain and IP lists:
   ```
   ./update
   ```

## Configuration Customization

### Changing DNS Servers

Edit `config.yaml` to modify the upstream DNS servers:

- `china_dns`: DNS servers for Chinese domains (default: AliDNS and DNSPod)
- `google_dns` and `cloudflare_dns`: DNS servers for foreign domains

### Modifying Routing Rules

The main routing logic is defined in the `resolve_process` sequence in `config.yaml`. You can adjust the rules to change how domains are routed.

### Custom Host Mappings

Add custom host mappings in the `hosts` file using the format:

```
192.168.1.100 myserver.local
```

## License

See the [LICENSE](LICENSE) file for details.

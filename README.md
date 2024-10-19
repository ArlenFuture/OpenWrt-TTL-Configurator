# OpenWrt-TTL-Configurator

This repository provides a script to set the TTL value on OpenWrt 23.05.5. The TTL (Time To Live) value is set to 65 to prevent ISP detection, especially useful in scenarios involving 4G or 5G network sharing.

## Contents
The script creates a rule for nftables to set the TTL for all outgoing packets to 65. This configuration is added to OpenWrt's startup process to ensure it applies every time the router boots.

## Installation Steps

### 1. Add the TTL setting to Startup
In the OpenWrt web interface (LuCI), go to:

`System -> Startup -> Local Startup`

Then add the following script to the Local Startup section:

```sh
# Create a directory for custom nftables rules
mkdir -p /usr/share/nftables.d/chain-pre/mangle_postrouting/

# Set TTL value to 65
echo "ip ttl set 65" > /usr/share/nftables.d/chain-pre/mangle_postrouting/01-set-ttl.nft

# Reload firewall rules
fw4 reload
```

### 2. Restart the router
After adding the script, restart the router to apply the TTL rule.

## Removal
To remove the TTL rule, delete the nftables rule file and reload the firewall:

```sh
rm /usr/share/nftables.d/chain-pre/mangle_postrouting/01-set-ttl.nft
fw4 reload
```

## Notes
This script has been tested only on OpenWrt 23.05.5, where nftables is guaranteed to be present.

## Contributions
Feel free to submit pull requests and suggestions. For issues or improvements, please open an issue on this repository.

## License
This project is licensed under the MIT License. See the LICENSE file for details.

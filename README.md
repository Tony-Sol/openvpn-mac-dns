# openvpn-mac-dns
Manage DNS settings pushed from OpenVPN

This script is the Mac equivalent of Debian's /etc/openvpn/update-resolv-conf . It is based on the same
code, but uses `networksetup` instead of `resolvconf` to perform the work.
The name is therefore slightly misleading (it does not touch /etc/resolv.conf),
but using the same name lets us write OS-agnostic "up" and "down" directives in the client configuration.

To use, install ./update-resolv-conf on the client Mac. You can then configure "up" and "down"
directives in the openvpn client config file as exampled:

```
script-security 2
up /usr/local/etc/openvpn/openvpn-mac-dns/update-resolv-conf
down /usr/local/etc/openvpn/openvpn-mac-dns/update-resolv-conf
```

Or equivalently, use as openvpn cli command arguments as follows:

```
sudo openvpn --config connection.ovpn --script-security 2 --up ~/.openvpn/update-resolv-conf --down ~/.openvpn/update-resolv-conf
```

In OpenVPN Access Server, set the following option in the server configuration to automatically
add the above to the generated client files:

```
"vpn.client.config_text": "up /etc/openvpn/update-resolv-conf\ndown /etc/openvpn/update-resolv-conf", 
```

Or equivalently, incant the following on the server command line:

```
/usr/local/openvpn_as/scripts/confdba -mk "vpn.client.config_text" -v "up /etc/openvpn/update-resolv-conf\ndown /etc/openvpn/update-resolv-conf"
```

# Credits

Adapted from the original script here: https://github.com/masterkorp/openvpn-update-resolv-conf

networksetup invocation borrowed from https://blog.netnerds.net/2011/10/openvpn-update-client-dns-on-mac-os-x-using-from-the-command-line/

Forked from https://github.com/andrewgdotcom/openvpn-mac-dns
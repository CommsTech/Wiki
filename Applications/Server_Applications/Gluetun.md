# Gluetun VPN Add-on Setup Guide

Basic setup of the [TrueCharts](https://www.truecharts.org/) [Gluetun](https://github.com/qdm12/gluetun/) VPN addon

## Prerequisites[​](https://docs.dasnipe.com/docs/truenas/Gluetun-Guide/#prerequisites "Direct link to Prerequisites")

- Anything migrated to the new common chart that features Gluetun
- Ideally a VPN provider supported by Gluetun, check the [Wiki](https://github.com/qdm12/gluetun/wiki) on the [Gluetun](https://github.com/qdm12/gluetun/) site for more info

## Gluetun VPN Addon Setup[​](https://docs.dasnipe.com/docs/truenas/Gluetun-Guide/#gluetun-vpn-addon-setup "Direct link to Gluetun VPN Addon Setup")

### OpenVPN[​](https://docs.dasnipe.com/docs/truenas/Gluetun-Guide/#openvpn "Direct link to OpenVPN")

- Install app as per usual and scroll down the to the `Addons` section
- Click on `VPN` and select `Gluetun`

![VPN Gluetun 1](https://docs.dasnipe.com/assets/images/Gluetun-VPN1-2fad7981c217e30633d8b32eb63f51c8.png)

`Gluetun` works with Environment Variables so we need to configure them below. Enter your `VPN Provider` specific ones (see blow)

- VPN Provider specific Env Vars

![VPN Gluetun 2](https://docs.dasnipe.com/assets/images/Gluetun-VPN2-b3acede361d99ca0d1733848691ae846.png)

- All providers will generally need `VPN_SERVICE_PROVIDER` and `VPN_TYPE`, for me it's `Windscribe` and `openvpn` but I could easily choose `Wireguard`
- Scroll to the [Gluetun Wiki](https://github.com/qdm12/gluetun/wiki) and find your specific provider and enter their info, eg [Windscribe Wiki Page](https://github.com/qdm12/gluetun/wiki/Windscribe)

### Wireguard[​](https://docs.dasnipe.com/docs/truenas/Gluetun-Guide/#wireguard "Direct link to Wireguard")

I will demonstrate using 'Mullvad' as the provider.

- I pull my private key, endpoint port and Wireguard Addresses from a Mullvad wireguard config file.

![Mullvad Config File](https://docs.dasnipe.com/assets/images/Gluetun-VPN4-c0ac7eac78ee72c9c3ec0c4370cd4b04.png)

- You can generate a new config file from the Mullvad website, here is the [Mullvad Config Generator](https://mullvad.net/en/account/#/wireguard-config/)

Now we can enter the Env Vars

- Install app as per usual and scroll down the to the `Addons` section, click `Add` for each new environment variable

![WG ENV Vars 1](https://docs.dasnipe.com/assets/images/Gluetun-VPN5-4382e73fa6e56418c44df1b33282998c.png)

- Enable the killswitch by ticking `Enable Killswitch` box
    
- Click `Add` for every subnet you would like to exclude from the VPN tunnel. I have added my local subnet.
    

> Specifying the kubernetes subnet is not necessary as it is automatically excluded from the VPN tunnel

- VPN Config File Location is not necessary, we will be using environment variables instead, so leave it blank

![WG ENV Vars 2](https://docs.dasnipe.com/assets/images/Gluetun-VPN6-651f9264d1abf47ddc6c54289482ac7e.png)

- `VPN_TYPE` is `wireguard`
- `VPN_SERVICE_PROVIDER` is `mullvad` in my case

![WG ENV Vars 3](https://docs.dasnipe.com/assets/images/Gluetun-VPN7-df1f0c39db0a589d38aff03fbc8c3f18.png)

- `WIREGUARD_PRIVATE_KEY` is the private key from the Mullvad config file above
- `FIREWALL_VPN_INPUT_PORTS` is the _port forward_ port, to forward a port with Mullvad, follow steps 2 and 3 from here: [Mullvad Port Forwarding](https://mullvad.net/en/help/port-forwarding-and-mullvad/)
- `WIREGUARD_ADDRESSES` is the Mullvad endpoint IP address, found in the Mullvad config file above

![WG ENV Vars 4](https://docs.dasnipe.com/assets/images/Gluetun-VPN8-75f3de9635817cea4628f67599fecd81.png)

- `SERVER_CITIES` is the Mullvad server city, it should likely be in from the same city your config file is from, and should share the same city as your forwarded port. In my case, I am using the `Toronto` server city, and my forwarded port is from `Toronto`.
    
- `VPN_ENDPOINT_PORT` is the Mullvad endpoint port, found in the Mullvad config file above
    

## Verify it works[​](https://docs.dasnipe.com/docs/truenas/Gluetun-Guide/#verify-it-works "Direct link to Verify it works")

Easiest way to verify after it deploys (the app will fail if your credentials don't work) for me is using `qbittorrent` since the network page shows the interfaces can be shown quickly (or check the logs)

![VPN Gluetun 2](https://docs.dasnipe.com/assets/images/Gluetun-VPN3-7db287e39e37ef0d7a2fef40489cfb6a.png)



## ProtonVPN Info

# ProtonVPN

## TLDR

```sh
docker run -it --rm --cap-add=NET_ADMIN -e VPN_SERVICE_PROVIDER=protonvpn \
-e OPENVPN_USER=abc -e OPENVPN_PASSWORD=abc \
-e SERVER_COUNTRIES=Netherlands qmcgaw/gluetun
```

```yml
version: "3"
services:
  gluetun:
    image: qmcgaw/gluetun
    cap_add:
      - NET_ADMIN
    environment:
      - VPN_SERVICE_PROVIDER=protonvpn
      - OPENVPN_USER=abc
      - OPENVPN_PASSWORD=abc
      - SERVER_COUNTRIES=Netherlands
```

💁 To use with Wireguard, download a configuration file from [account.proton.me/u/0/vpn/WireGuard](https://account.proton.me/u/0/vpn/WireGuard) and head to [the custom provider Wireguard section](custom.md#wireguard). Thanks to [@pvanryn](https://github.com/pvanryn) for pointing this out. Note however you cannot filter servers as easily as with OpenVPN since each server uses its own private key and/or peer address.

## Required environment variables

- `VPN_SERVICE_PROVIDER=protonvpn`
- `OPENVPN_USER` is your **OPENVPN specific** username. Find it at [account.proton.me/u/0/vpn/OpenVpnIKEv2](https://account.proton.me/u/0/vpn/OpenVpnIKEv2).
- `OPENVPN_PASSWORD`

## Optional environment variables

- `SERVER_COUNTRIES`: Comma separated list of countries
- `SERVER_REGIONS`: Comma separated list of regions
- `SERVER_CITIES`: Comma separated list of cities
- `SERVER_HOSTNAMES`: Comma separated list of server hostnames
- `FREE_ONLY`: Filter only free tier servers by setting it to `on`. It defaults to `off`.
- `VPN_ENDPOINT_PORT`: Custom OpenVPN server endpoint port to use
  - For TCP: `443`, `5995` or `8443`
  - For UDP: `80`, `443`, `1194`, `4569`, `5060`
  - Defaults are `1194` for UDP and `443` for TCP
- `VPN_PORT_FORWARDING`: defaults to `off` and can be set to `on`to enable port forwarding on the VPN server. For Wireguard, additionally set `VPN_PORT_FORWARDING_PROVIDER=protonvpn`.

## VPN server port forwarding

Requirements:

- `VPN_PORT_FORWARDING=on`
- Pick a VPN server which supports 'P2P', see step 1 on [this page](https://protonvpn.com/support/port-forwarding-manual-setup/). This will be partly automated for OpenVPN with [#1582](https://github.com/qdm12/gluetun/issues/1582).
- If you use **Wireguard** using the custom provider, set `VPN_PORT_FORWARDING_PROVIDER=protonvpn`

## Multi hop regions

Simply set the `SERVER_HOSTNAMES` environment variable to a hostname corresponding to a multi hop region (see [Servers](#servers)).

For example setting `SERVER_HOSTNAMES=ch-us-01a.protonvpn.com` would set a multi hop with entry in Switzerland and exit in the US.

## Moderate NAT/NAT Type 2

Paid ProtonVPN subscribers can optionally use [Moderate NAT](https://protonvpn.com/support/moderate-nat/) on their connections.

To do so, the OpenVPN username assigned by ProtonVPN should have `+nr` appended to the end of it.

## Servers

To see a list of servers available, [list the VPN servers with Gluetun](../servers.md#list-of-vpn-servers).
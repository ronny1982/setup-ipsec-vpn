# IPsec VPN Server Auto Setup Scripts

Set up your own IPsec VPN server in just a few minutes, with both IPsec/L2TP and Cisco IPsec on Ubuntu, Debian and CentOS. All you need to do is provide your own VPN credentials, and let the scripts handle the rest.

An IPsec VPN encrypts your network traffic, so that nobody between you and the VPN server can eavesdrop on your data as it travels via the Internet. This is especially useful when using unsecured networks, e.g. at coffee shops, airports or hotel rooms.

We will use <a href="https://libreswan.org/" target="_blank">Libreswan</a> as the IPsec server, and <a href="https://github.com/xelerance/xl2tpd" target="_blank">xl2tpd</a> as the L2TP provider.

## Quick start

First, prepare your Linux server[\*](#quick-start-note) with a fresh install of Ubuntu LTS or Debian.

Use this one-liner to set up an IPsec VPN server:

```bash
sudo bash <(curl -sL https://git.io/vpnsetup-interactive)
```

For other installation options and how to set up VPN clients, read the sections below.

<a name="quick-start-note"></a>
\* A dedicated server or virtual private server (VPS). OpenVZ VPS is not supported.

## Features

- **New:** The faster `IPsec/XAuth ("Cisco IPsec")` mode is supported
- Automated IPsec VPN server setup, with interactive user input
- Encapsulates all VPN traffic in UDP - does not need ESP protocol
- Can be directly used as "user-data" for a new Amazon EC2 instance
- Includes `sysctl.conf` optimizations for improved performance

## Requirements

A newly created <a href="https://aws.amazon.com/ec2/" target="_blank">Amazon EC2</a> instance, from one of these images:
- <a href="https://cloud-images.ubuntu.com/locator/" target="_blank">Ubuntu 20.04 (Focal), 18.04 (Bionic) or 16.04 (Xenial)</a>
- <a href="https://wiki.debian.org/Cloud/AmazonEC2Image" target="_blank">Debian 10 (Buster)</a>[\*](#debian-10-note)<a href="https://wiki.debian.org/Cloud/AmazonEC2Image" target="_blank">, 9 (Stretch) or 8 (Jessie)</a>

Please see <a href="https://blog.ls20.com/ipsec-l2tp-vpn-auto-setup-for-ubuntu-12-04-on-amazon-ec2/#vpnsetup" target="_blank">detailed instructions</a> and <a href="https://aws.amazon.com/ec2/pricing/" target="_blank">EC2 pricing</a>.

**-OR-**

A dedicated server or KVM/Xen-based virtual private server (VPS), freshly installed with one of the above OS. OpenVZ VPS is not supported, users could instead try <a href="https://github.com/Nyr/openvpn-install" target="_blank">OpenVPN</a>.

This also includes Linux VMs in public clouds, such as <a href="https://blog.ls20.com/digitalocean" target="_blank">DigitalOcean</a>, <a href="https://blog.ls20.com/vultr" target="_blank">Vultr</a>, <a href="https://blog.ls20.com/linode" target="_blank">Linode</a>, <a href="https://cloud.google.com/compute/" target="_blank">Google Compute Engine</a>, <a href="https://aws.amazon.com/lightsail/" target="_blank">Amazon Lightsail</a>, <a href="https://azure.microsoft.com" target="_blank">Microsoft Azure</a>, <a href="https://www.ibm.com/cloud/virtual-servers" target="_blank">IBM Cloud</a>, <a href="https://www.ovh.com/world/vps/" target="_blank">OVH</a> and <a href="https://www.rackspace.com" target="_blank">Rackspace</a>.

<a href="azure/README.md" target="_blank"><img src="docs/images/azure-deploy-button.png" alt="Deploy to Azure" /></a> <a href="http://dovpn.carlfriess.com/" target="_blank"><img src="docs/images/do-install-button.png" alt="Install on DigitalOcean" /></a> <a href="https://cloud.linode.com/stackscripts/37239" target="_blank"><img src="docs/images/linode-deploy-button.png" alt="Deploy to Linode" /></a>

<a href="https://blog.ls20.com/ipsec-l2tp-vpn-auto-setup-for-ubuntu-12-04-on-amazon-ec2/#gettingavps" target="_blank">**&raquo; I want to run my own VPN but don't have a server for that**</a>

Advanced users can set up the VPN server on a $35 <a href="https://www.raspberrypi.org" target="_blank">Raspberry Pi</a>. See <a href="https://blog.elasticbyte.net/setting-up-a-native-cisco-ipsec-vpn-server-using-a-raspberry-pi/" target="_blank">[1]</a> <a href="https://www.stewright.me/2018/07/create-a-raspberry-pi-vpn-server-using-l2tpipsec/" target="_blank">[2]</a>.

<a name="debian-10-note"></a>
\* Debian 10 users should use the standard Linux kernel (not the "cloud" version). Read more <a href="docs/clients.md#debian-10-kernel" target="_blank">here</a>.   

:warning: **DO NOT** run these scripts on your PC or Mac! They should only be used on a server!

## Installation

### Ubuntu & Debian

First, update your system with `apt-get update && apt-get dist-upgrade` and reboot. This is optional, but recommended.

To install the VPN, please choose one of the following options:

**Option 1:** Have the script generate random VPN credentials for you (will be displayed when finished):

```bash
wget https://git.io/vpnsetup-interactive -O vpnsetup.sh && sudo sh vpnsetup.sh
```

**Option 2:** Edit the script and provide your own VPN credentials:

```bash
wget https://git.io/vpnsetup-interactive -O vpnsetup.sh
nano -w vpnsetup.sh
[Replace with your own values: YOUR_IPSEC_PSK, YOUR_USERNAME and YOUR_PASSWORD]
sudo sh vpnsetup.sh
```

**Note:** A secure IPsec PSK should consist of at least 20 random characters.

**Option 3:** Define your VPN credentials as environment variables:

```bash
# All values MUST be placed inside 'single quotes'
# DO NOT use these special characters within values: \ " '
wget https://git.io/vpnsetup-interactive -O vpnsetup.sh && sudo \
VPN_IPSEC_PSK='your_ipsec_pre_shared_key' \
VPN_USER='your_vpn_username' \
VPN_PASSWORD='your_vpn_password' \
sh vpnsetup.sh
```

**Note:** If unable to download via `wget`, you may also open <a href="vpnsetup.sh" target="_blank">vpnsetup.sh</a> and click the **`Raw`** button. Press `Ctrl-A` to select all, `Ctrl-C` to copy, then paste into your favorite editor.

## Next steps

Get your computer or device to use the VPN. Please refer to:

<a href="docs/clients.md" target="_blank">**Configure IPsec/L2TP VPN Clients**</a>

<a href="docs/clients-xauth.md" target="_blank">**Configure IPsec/XAuth ("Cisco IPsec") VPN Clients**</a>

<a href="docs/ikev2-howto.md" target="_blank">**Step-by-Step Guide: How to Set Up IKEv2 VPN**</a>

If you get an error when trying to connect, see <a href="docs/clients.md#troubleshooting" target="_blank">Troubleshooting</a>.

Enjoy your very own VPN! :sparkles::tada::rocket::sparkles:

## Important notes

**Windows users**: This <a href="docs/clients.md#windows-error-809" target="_blank">one-time registry change</a> is required if the VPN server and/or client is behind NAT (e.g. home router).

**Android users**: If you encounter connection issues, try <a href="docs/clients.md#android-mtumss-issues" target="_blank">these steps</a>.

The same VPN account can be used by your multiple devices. However, due to an IPsec/L2TP limitation, if you wish to connect multiple devices simultaneously from behind the same NAT (e.g. home router), you must use only <a href="docs/clients-xauth.md" target="_blank">IPsec/XAuth mode</a>.

If you wish to add, edit or remove VPN user accounts, see <a href="docs/manage-users.md" target="_blank">Manage VPN Users</a>. Helper scripts are included for convenience.

For servers with an external firewall (e.g. <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html" target="_blank">EC2</a>/<a href="https://cloud.google.com/vpc/docs/firewalls" target="_blank">GCE</a>), open UDP ports 500 and 4500 for the VPN. Aliyun users, see [#433](https://github.com/hwdsl2/setup-ipsec-vpn/issues/433).

Clients are set to use <a href="https://developers.google.com/speed/public-dns/" target="_blank">Google Public DNS</a> when the VPN is active. If another DNS provider is preferred, replace `8.8.8.8` and `8.8.4.4` in both `/etc/ppp/options.xl2tpd` and `/etc/ipsec.conf`, then reboot your server.

Using kernel support could improve IPsec/L2TP performance. It is available on Ubuntu 16.04-20.04 and Debian 9-10. Ubuntu users: Install `linux-modules-extra-$(uname -r)` (or `linux-image-extra`), then run `service xl2tpd restart`.

To modify the IPTables rules after install, edit `/etc/iptables.rules` and/or `/etc/iptables/rules.v4` (Ubuntu/Debian). Then reboot your server.

When connecting via `IPsec/L2TP`, the VPN server has IP `192.168.42.1` within the VPN subnet `192.168.42.0/24`.

The scripts will backup existing config files before making changes, with `.old-date-time` suffix.

## Upgrade Libreswan

The additional scripts <a href="extras/vpnupgrade.sh" target="_blank">vpnupgrade.sh</a> can be used to upgrade <a href="https://libreswan.org" target="_blank">Libreswan</a> (<a href="https://github.com/libreswan/libreswan/blob/master/CHANGES" target="_blank">changelog</a> | <a href="https://lists.libreswan.org/mailman/listinfo/swan-announce" target="_blank">announce</a>). Edit the `SWAN_VER` variable as necessary. Check which version is installed: `ipsec --version`.

```bash
# Ubuntu & Debian
sudo bash <(curl -sL https://git.io/vpnupgrade)
```

## Uninstallation

Please refer to <a href="docs/uninstall.md" target="_blank">Uninstall the VPN</a>.

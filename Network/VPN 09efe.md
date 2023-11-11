# VPN

```bash
apt-get update && apt-get upgrade
```

## Open VPN

You can use this repository for configuring the VPN.

[https://github.com/Nyr/openvpn-install](https://github.com/Nyr/openvpn-install)

```bash
# To get the script
wget https://github.com/Nyr/openvpn-install/raw/master/openvpn-install.sh

# Run the bash
sudo bash openvpn-install.sh

# Change the owner
sudo chown ubuntu safe-zone.ovpn

# Change configuration
# Change verb to 0
sudo vim /etc/openvpn/server/server.conf

# Start the vpn 
sudo systemctl restart openvpn-server@server.service

# Change the hostname
sudo hostnamectl set-hostname spcu
```

If you want to use this VPN for all of your traffic, `Tunnelblick` on mac OS, `openvpn` on windows OS, or load it into network manager in linux OS.

You can use `mosh` instead of ssh.

### Two Factor Authentication

If you want to be fantasy, you can use 2 factor authentication.

```bash
# Install
sudo apt install libpam-google-authenticator

# Initialize
google-authenticator

# Change configurations for sshd
vim /etc/pam.d/sshd
# Comment @include common-auth
# Add line bellow
auth required pam_google_authenticator.so

# Change the configrations of ssh
vim /etc/ssh/sshd_config
# ChallengeResponseAuthentication yes
# UsePAM yes
# Add line bellow
AuthenticationMethods publickey,password publickey,keyboard-interactive
```

To tunnel your system you can use this command

```bash
openvpn --config <YOUR_OVPN_FILE>
```

# References

[How to Make Your Own VPN (And Why You Would Want to)](https://www.youtube.com/watch?v=gxpX_mubz2A)
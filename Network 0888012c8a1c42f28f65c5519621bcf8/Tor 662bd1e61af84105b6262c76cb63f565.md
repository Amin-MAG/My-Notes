# Tor

```bash
# Install the tor
sudo apt install tor
```

## Setup Dark Web

```bash
# Change the tor configuration file
# Uncomment the hidden service
sudo vim /etc/tor/torrc

# Restart the service
sudo service tor restart
```
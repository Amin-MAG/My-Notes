---
title: mitmproxy
draft: true
tags: [networking, security, devops, reference]
---

# mitmproxy

An open-source **man-in-the-middle proxy** for intercepting, inspecting, and modifying HTTP/HTTPS traffic.

## Interfaces

|Tool|Type|Use|
|---|---|---|
|`mitmproxy`|TUI|Live interactive inspection|
|`mitmweb`|Web UI|Browser-based GUI|
|`mitmdump`|CLI|Scripting and logging|

## Start

```bash
mitmproxy        # TUI on :8080
mitmweb          # Web UI on :8081
mitmdump -w dump.mitm  # save session to file
```

## Shell Session Setup

```zsh
# ~/.zshrc

proxy-on() {
  export HTTP_PROXY=http://127.0.0.1:8080
  export HTTPS_PROXY=http://127.0.0.1:8080
  export http_proxy=http://127.0.0.1:8080
  export https_proxy=http://127.0.0.1:8080
  export NO_PROXY=localhost,127.0.0.1
  export NODE_EXTRA_CA_CERTS=~/.mitmproxy/mitmproxy-ca-cert.pem
  export REQUESTS_CA_BUNDLE=~/.mitmproxy/mitmproxy-ca-cert.pem
  export CURL_CA_BUNDLE=~/.mitmproxy/mitmproxy-ca-cert.pem
  export SSL_CERT_FILE=~/.mitmproxy/mitmproxy-ca-cert.pem
  echo "mitmproxy ON"
}

proxy-off() {
  unset HTTP_PROXY HTTPS_PROXY http_proxy https_proxy NO_PROXY \
        NODE_EXTRA_CA_CERTS REQUESTS_CA_BUNDLE CURL_CA_BUNDLE SSL_CERT_FILE
  echo "mitmproxy OFF"
}
```

## CA Certificate

mitmproxy generates a local CA at `~/.mitmproxy/` on first run. Install it to decrypt HTTPS.

**macOS (system-wide — remove when done):**

```bash
sudo security add-trusted-cert -d -r trustRoot \
  -k /Library/Keychains/System.keychain \
  ~/.mitmproxy/mitmproxy-ca-cert.pem
```

**Remove:**

```bash
sudo security delete-certificate -c "mitmproxy" /Library/Keychains/System.keychain
```

**Debian (system-wide — remove when done):**

```bash
sudo mkdir -p /usr/local/share/ca-certificates/
sudo cp ~/.mitmproxy/mitmproxy-ca-cert.pem /usr/local/share/ca-certificates/mitmproxy.crt
sudo update-ca-certificates
```

**Remove:**

```bash
sudo rm /usr/local/share/ca-certificates/mitmproxy.crt && sudo update-ca-certificates --fresh
```

> ⚠️ A trusted root CA is powerful — remove it after debugging.

## Python Addon Example

```python
# script.py
def request(flow):
    flow.request.headers["X-Debug"] = "1"

def response(flow):
    print(flow.request.url, flow.response.status_code)
```

```bash
mitmdump -s script.py
```

## Go HTTP Client

```go
client := &http.Client{
    Transport: &http.Transport{
        Proxy: http.ProxyFromEnvironment, // reads HTTP_PROXY / HTTPS_PROXY
    },
}
```
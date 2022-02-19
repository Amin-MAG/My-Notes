# EMQX

## Some useful HTTP API

```bash
# To publish a topic
# You can give more option like client ID or...
curl -i --basic -u admin:public -X POST "http://194.5.193.113:8081/api/v4/mqtt/publish" -d '{"topic":"led_right","payload":"Hello World"}'
```

# **Create Root Certificate**

**The following command creates the private key file.**

```bash
openssl genrsa -out root.key 2048
```

**To create a password-protected key by adding -des3.**

```bash
openssl genrsa -des3 -out root.key 2048
```

Then:

```bash
openssl req -new -key root.key -out root.csr
```

t**he above command will prompt for details.**

**You can use the above two files to sign the certificate.**

```bash
openssl x509 -req -days 365 -sha1 -extensions v3_ca -signkey root.key -in root.csr -out root.crt
```

# Creating an MQTT Server certificate

**You need to create the server key file using the following command.**

```bash
openssl genrsa -out server.key 2048
```

**Create a Server CSR file that holds the complete server details of the host. The following command will prompt for the company details.**

```bash
openssl req -new -key server.key -out server.csr
```

**Use the following command to create the Server certificate. Use the root certificate to create the server certificate.**

```bash
openssl x509 -req -days 365 -sha1 -extensions v3_req -CA root.crt -CAkey root.key -CAcreateserial -in server.csr -out server.crt
```

# **Creating MQTT Client certificate**

**The above procedure followed for the server certificate can be used to create the client certificates. Please use the appropriate name for the files.**

**The above certificates are also valid for 365 days. Same Certificate Authority is used for generating both the client and server certificate.**

# Configuration file

`emqx.conf` to update our certificates.
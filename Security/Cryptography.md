# Cryptography

## AES

Advanced Encryption Standard also known by its original name `Rijndael`. For AES, NIST selected three members of the `Rijndael` family, each with a block size of 128 bits, but three different key lengths: 128, 192 and 256 bits.

NITS is National Institute of Standards and Technology.

```bash
# Encrypt the ./file
# -e to encrypt
# -in for input file
# -out for output file
# -k for the key
openssl aes-256-cbc -e -in ./file -out ./encrypted -k paLUGDS8TCyVbqh3

# Decrypt the ./encrypted 
openssl aes-256-cbc -d -in ./encypted -k paLUGDS8TCyVbqh3 >> file
```

## X509

It is a standard for defining the format of public key certificates.

Keep in the mind that use different names for the common name.

```bash
# To create both private key and certificate at the same time.
# The vertificate is valid for 365 days
# You can pass the certificate information with -subj
openssl req -x509 -newkey rsa:4096 -days 365 -keyout ca-key.pem -out ca-cert.pem

# To display certificate information
openssl x509 -in ca-cert.pem -noout -text
```

## Hashing

To generate an MD5 hash based on the input text

```bash
# It is a cryptographic hash: static length and looks random
md5sum
here_is_my_plain_text
^D
```

# Signing

## Signing the certificate request

```bash
# Get signed certificate
openssl x509 -req -in server-req.pem -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem
```

## Verify Signing

```bash
openssl verify -CAfile ca-cert.pem server-cert.pem
```

# See more

- [Hashes](Hash.md)

# References

[Advanced Encryption Standard - Wikipedia](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
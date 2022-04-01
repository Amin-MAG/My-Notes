# Security

### Stack Overflow
Usually, Stacks are much smaller than heaps. For getting a stack overflow error, you can do a couple of things, For example calling a function recursively or defining a significant variable in a line.

```c
#include <stdio.h>

int main(){
	printf("Try to stack overflow...\n");
	main();
	return 0;
}
```

If you run this code, you will see that at the end, it shows a Segmentation fault (core dumped) error (That is, the stack overflow error).

### Buffer Overflow

On the other hand, some functions like `gets()` are not safe. They don't check the input length and replace them in the stack. They have some side effects because of this buffer overflow issue. Assume there are two variables next to each other in the stack, and one of them is the cipher, and the other is your guess (program input). If the program uses `gets()` to get the string from the user, the user can profit from overwriting the cipher by giving a larger string than the program expects. 

# Digital signature

## Purposes

1. Authentication: To prove that the message was created and sent by the claimed server.
2. Non-Repudiation: The sender can't deny having sent the message later on.
3. Integrity: To ensure that the message was not altered in transmission.

### Asymmetric cryptography

# Certificates

## Why use Certificates?

Assume we want to establish an encrypted connection between client and server. We need to have the same key on the client-side and server-side. But there is no way that we can create it directly.

### Encrypting messages

The solution is using a private and a public key. If you use the private key to encrypt, you can use the public key to decrypt. Vice versa, you can use the public key to encrypt, then you can decode using the private key.

When a client wants to connect to a server, first asks for a public key. The server will send its public key to the client. The client uses the key to encrypt its own key. So now we have The client key encrypted by the server's public key. The client will send this encrypted key to the server. For sniffing, you need the private key to decrypt while the server is the only one that has the key. So the server receives the encrypted client key, decrypts it using the private key. Now both the server and client have the same key. They can encrypt and decrypt messages to each other. As there is no way to access the client key by sniffing through this process, the communication remains safe.
So why do we use certificates?

### Why do we need certificates?

To be honest, there is a way for sniffing. What if a person intercepts the server and sends its own public key instead of the server public key? In this case, the person can decrypt to catch the client key by using the private key. (The key can be encrypted with the server public key and be sent)
To wrap up, we should make sure that we receive the response from the correct server.

The solution is using certificate authority which is a third party. The server sends the public key to the certificate authority and wants to sign it. The certificate authority will return a certificate. A certificate contains the domain, the server public key, and the signature. The signature itself is the server public key encrypted by the private key of the certificate authority.

The certificate authority also needs an intermediate certificate that is linked to the former certificate. As you see, there is a chain of certificates until we get to the root certificate. The root certificates are self-signed, and it is installed on Operating systems. So all the clients have those root certificates.

Before establishing any connection with the server, The server sends the certificate to the client. The client will see what certificate authority it is. Then By using the certificate authority public key, The client can encrypt the public key and check if it is equal to the signature. It Also can decrypt the signature and check if it is similar to the server public key or not. It is continuing until it gets to the root certificate, which the client trusts.

## Certificate authority

As it was described in the "Why use certificate?" section, we need a third-party solution that both our server and clients can trust. 

## Resources

[What are SSL/TLS Certificates? Why do we Need them? and How do they Work?](https://www.youtube.com/watch?v=r1nJT63BFQ0&t=627s)

[Certificates and Certificate Authority Explained](https://www.youtube.com/watch?v=x_I6Qc35PuQ&t=1s)

# OpenSSL Command

OpenSSL is a CLI application for decryption and encryption.

## Get started

To see all kinds of cipher encryptions.

```bash
$ openssl list -cipher-commands
```

As an example if you want to encrypt a file `msg` you can execute this command:

```bash
$ openssl enc -aes-256-cbc -base64 -in msg
enter aes-256-cbc encryption password:
Verifying - enter aes-256-cbc encryption password:
U2FsdGVkX1/fTtG/tSLZKXDpAAuHkaSfELOBOLqC7QVrcKFEbmhW12WC6ZvUt1+V
```

Here it encrypts the input file (`-in`) by using AESE-256-CBC and in the format of BASE64. The output is going to print in the terminal. you can use `-out` to create a new encrypted file.

```bash
openssl enc -aes-256-cbc -base64 -in msg -out output
```

So if we want to decrypt this message we should use `-d` flag.

```bash
$ openssl enc -aes-256-cbc -d -base64 -in enc
enter aes-256-cbc decryption password:
hello my name is amin
```

## RSA

To create a new private RSA key

```bash
# Generate RSA
$ openssl genrsa -out keypairA.pem 2048
```

It will generate an RSA key with a bit length of 2048. 

You can use these keys to do some other operations and commands. For example for printing out all details about these keys including both the private and the public key you can use this command:

```bash
# Show details
$ openssl rsa -in keypairB.pem -text
```

To generate a public key for a private key:

```bash
# Generate public key
$ openssl rsa -in keypairA.pem -pubout -out publicA.pem
```

Now let's think that we have 2 points A and B. Both of them have a private key and a public key. To encrypt a message with B's public key we need to execute these commands:

```bash
$ openssl rsautl -encrypt -in msg -out enc -inkey publicB.pem -pubin
```

`rsautl` is actually the RSA utility command. it uses `publicB.pem` to encrypt the message. `-pubin` is essential because sometimes the `-inkey` is a certificate. So in that case we use `-certin` instead of `-pubin`.

Then we send this message to B. B decrypts the message by executing:

```bash
$ openssl rsautl -decrypt -in received -out msg -inkey keypairB.pem
```

This time let's sign the message in A and then pass it to B:

```bash
$ openssl rsautl -sign -ing msg -out signed -inkey keypairA.pem
```

B receives the signed message and wants to verify if it is encrypted by A or not.

```bash
$ openssl rsautl -verify -in signed -out msg -inkey publicA.pem -pubin
```

If we encrypt the `keypairA.pem` then for any `rsautl` commands it will ask for a passphrase:

```bash
# Encrypt the private key
$ openssl rsa -in keypairA.pem -des3 -out privateA.pem

# Then we can use `privateA.pem` instead, but it will ask for passphrase each time.
# so it is more secure.
$ openssl rsautl -sign -ing msg -out signed -inkey keypairA.pem
Enter passphrase:

```

## Others

```bash
$ openssl req -new -key root.key -out root.csr
```

```bash
$ openssl x509 -req -days 365 -sha1 -extensions v3_ca -signkey root.key -in root.csr -out root.crt
```

```bash
$ openssl x509 -req -days 365 -sha1 -extensions v3_req -CA root.crt -CAkey root.key -CAcreateserial -in server.csr -out server.crt
```

## Resources

[Encryption and decryption with openssl](https://www.youtube.com/watch?v=-nEh7X4dtuw)

# CORS

It stands for Cross-origin resource sharing. It is a kind of browser security feature that you can specify certain origins that are allowed to access your APIs. CORS is just for browsers, so if you try to access the APIs using a terminal, it is ok.

So there is no complexity here, the browser will check the access-control-allowed-origins and will get data without error if the origin is allowed.

[Cryptography](Cryptography.md)

# Resources
- [Stack Overflow - Jadi](https://www.youtube.com/watch?v=RLlQFfZoEB8)
- [Buffer Overflow - Jadi](https://www.youtube.com/watch?v=xjRFubg5Ghs)
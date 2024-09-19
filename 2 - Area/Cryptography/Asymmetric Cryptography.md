Asymmetric key cryptography uses a pair of mathematically linked public(encryption key) and private key(decryption key).

The public key is shared with everyone while private key is secret.

### Encryption

- Bob share the public key to everybody, and keeps the private key secret.
- Anyone who needs to send message to Bob, uses the public key to encrypt the message
- Only Bob can decrypt the message using his private key.

Hence secure communication over insecure channel

### Authentication
- Bob share public key to everybody, and keeps the private key secret.
- Bob signs the message with the private key
- Anyone with public key can now verify that the original message was signed by Bob.


### ECDSA

The s in https stands for TLS encryption. This means that the browser connects to the website over a encrypted connection. The browser also validates the website using public key cryptography and digital certificate


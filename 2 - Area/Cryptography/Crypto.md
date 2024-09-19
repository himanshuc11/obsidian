Cryptography is used to provide security and protection to the information. 
Cryptography deals with storing and transmitting data in a secured way.

Broadly there are two types of algorithms
- Symmetric Key: Same key used for encryption and decryption. ex AES, TwoFish, ChaCha20
- Asymmetric Key: Uses public key (encryption key) and private key(decryption key)

These keys are large secret numbers derived from key derivation algorithms like PBKDF2 and scrypt.

### Digital Signatures
Cryptography allows digital signing of messages which guarantees message authenticity and integrity. Most algorithms use Asymmetric key encryption in which the message is **signed by the private key** and **verified by the public key**.

### Message Authentication
Message authentication algorithms like **HMAC** and message authentication codes like **MAC codes** to prove message authenticity, integrity and authorship.

### Key Exchange

The process of calculating the value of hash function is hashing.
Cryptographic hash functions are functions that take in input and generate a fixed size output

![[Screenshot 2024-07-25 at 11.35.13 AM.png]]Example: SHA 256, SHA3 256 generate 256 bits output.
The length of output depends on the algorithm.

### Properties of a good hash function

- **Deterministic**
  Same message input given to the hash function, the output hash generated should be the same.
- **One way**
  Generating a valid input message from the output hash should be infeasible. The only way to find the input message should be to generate hashes for all possible inputs and compare (Brute force)
- **Pseudo Random**
  Minor difference in input should lead to large difference in hash output. Idea being prevent brute force attacks.
- **Collision resistant**
  A **collision** means that two different inputs generate the same output hash. A good hash function should be collision resistant.
- **Fast to compute**

### Uses
- Verify integrity of files, docs, messages etc. We use 256 checksum to ensure that the file has not been modified after release
- Storing password in database using hash, instead of plain text passwords
- Generate unique Id of a certain document. ex Git uses SHA 1 to uniquely identify a commit.
- Pseudo random number generator
- Proof of work algorithms

### Secure Hash Algorithms

Insecure Algorithms
- SHA1
- MD5

SHA (Secure Hash Functions)
- SHA2
- SHA 256
- SHA 512

More hash bits, stronger security. Thus SHA 512 is more unlikely to find a collision than SHA 256.

SHA3 (Keccak Family of hashes)
- SHA 3
- SHA3 256
- SHA3 512

SHA3 are more secure than SHA2 for the same hash length.
Kekkak-256 is a hashing algorithm used in Ethereum, which is a variant of SHA3 - 256

### JS Code

**Generate Hash**
```js
const { sha256 } = require("ethereum-cryptography/sha256");
const { utf8ToBytes } = require("ethereum-cryptography/utils");

const message = "hello world";
const messageBytes = utf8ToBytes(message);
const hash = sha256(messageBytes);
```

**Compare Hash**
```js
const { toHex } = require("ethereum-cryptography/utils");

function comapreHashes(hash1, hash2) {
	return toHex(hash1) === toHex(hash2)
}
```


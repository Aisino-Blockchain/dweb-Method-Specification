# dweb-Method-Specification
## Abstract
dweb is a decentralized network system for building self-sovereign identities and verifiable claims. It is implemented based on blockchain technology. Dids (Decentralized Identifiers) are used to identify entities (people, institutions, things, etc.). Dids and VC provided by DWEB can realize distributed interconnection among independent business systems and easily build trusted data flow channels. Support for trusted flow of entity ID, entity credentials/identity data across systems, organizations, and geographies.
## Status of this document
This is a draft document and will be updated based on W3C.
## DID Method Name
The name string that shall identify this DID method is: `dweb`
## DID Format
```
# dweb
"did:dweb:" + namespace-id + ":" + dweb-identifier
# namespace-id
less than 10 characters composed of lowercase letters and numbers
# dweb-identifier
dweb-identifier = 1* idchar idchar = 1-9 / A-H / J-N / P-Z / a-k / m-z

Example did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr
```
## DID Document
Example did document of `did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr` is
```
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr",
  "controller": "did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr",
  "created": 1642992401,
  "updated": null,
  "verificationMethod": [
    {
      "id": "did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr#keys-0",
      "type": "Secp256k1",
      "controller": "did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr",
      "publicKey": "9140759724994454628181963565667341739731028461151630822625149865018431527829307192022895075113804689383949423804960687290646469426121233393934795192655714"
    }
  ],
  "authentication": [
       "did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr#keys-0"
  ],
  "service": []
}
```
## CRUD Operations
did:dweb method (CRUD) operations are provided in the form of apis. DID documents are generated in the blockchain via smart contract code. It checks permissions in the smart contract and performs signature verification.
### Create
DID generation is requested with a simple input. The input values are shown below. If the creation succeeds, "true" is returned.
```
method: did_create
input : {did , publicKey , signature}
output: {result}
```
example input data
```
{
   "id":"did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr",
   "publicKey": "9140759724994454628181963565667341739731028461151630822625149865018431527829307192022895075113804689383949423804960687290646469426121233393934795192655714",
   "signature":"5nbzWuDGXyb...FPg6HmZ4JM6q=="
}
```
example output data
```
{
"result"  : true
}
```
### Read
To read the did document, the input values must be as follows. If it is found, it returns a did document.
```
method: did_read
input : {did}
output: {did_document}
```
example input data
```
{
"id": "did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr"
}
```
example output data
```
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr",
  "controller": "did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr",
  "created": 1642992401,
  "updated": null,
  "verificationMethod": [
    {
      "id": "did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr#keys-0",
      "type": "Secp256k1",
      "controller": "did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr",
      "publicKey": "9140759724994454628181963565667341739731028461151630822625149865018431527829307192022895075113804689383949423804960687290646469426121233393934795192655714"
    }
  ],
  "authentication": [
       "did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr#keys-0"
  ],
  "service": []
}
```
### Update
DID documents do not support updates.
### Delete
```
method: did_delete
input : {did,signature}
output: {result}
```
example input data
```
{
   "id":"did:dweb:dtds-1:4WERTYKJHGFC4569RFB38E719nmDSAk961Kr",
   "signature":"5nbzWuDGXyb...FPg6HmZ4JM6q=="
}
```
example output data
```
{
"result": true
}
```
## Security considerations
### Eavesdropping
Given that all data are public and stored on the blockchain, the risk of unauthorized data interception (eavesdropping) is mitigated as the data is not sensitive by design.
### Replay Attacks
To prevent replay attacks, did proofs must have a random nonce value. In a decentralized network or smart contract, the code cannot generate reliable random numbers. To solve this problem, we store the nonce value and check if the value is already used.
### Message Insertion, Deletion, Modification
The authorization verification mechanism is provided through the DID controller. The DID controller needs to sign the relevant operations, and this signature needs to be verified when the operation is executed to prove that the relevant operation is an authorized operation. 
### Denial of Service (DoS)
The decentralized nature of blockchain significantly limits the impact of DoS attacks. Rate controls and request limitations can further mitigate this risk.
- The use of high-speed, high-security elliptic curve cryptography enables a typical CPU to perform public-key operations faster than those required by a typical Internet connection. The server does not allocate memory until the client sends the initialization packet.
- For the read request, because multi-signature state proof is adopted, only one ledger node needs to be communicated to obtain the real ledger data
### Residual Risks
Residual risks for dweb include:
- compromise in the used cryptographic primitives (BLS-Signature, X25519, Ed25519 etc.)
- implementation bugs in the blockchain software, especially as all nodes use the same software package
- external libraries
### Integrity Protection and Update Authentication
All operations require a signature from the DID controller, providing both authentication and integrity protection.
### Man-In-The-Middle
During the communication between the client and the blockchain node, the client obtains the public key of the ledger node from the public genesis block. The client authenticates the sensitive write request with a signature with a verification key at the application layer. Since both communicating parties know each other's public key, it is impossible to perform man-in-the-middle attacks on inter-node communication.
### Policy for Unique Assignment
The user creates a public-private key pair by entering a seed, and the private key is hosted in an agent provided by DWEB.
The DID identifier is calculated from the public key using an address translation algorithm, similar to the account address of Ethereum.

## Privacy Considerations
The blockchain used by DWEB and the DID documents do not contain personally identifiable information.

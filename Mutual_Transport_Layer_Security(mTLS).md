## TLS
TLS or Transport Layer Security was a protocol invented to allow encrypted traffic to flow around the web. Traditional HTTP which uses TCP as a means to send data across the internet doesn't care about authentication or encryption. This means it is incredibly unsafe and liable to attack from hackers. 

## TLS Handshake
The TLS handshake is the way two systems exchange information to open secure channel of communication. It follows a set of steps to ensure encryption occurs.
1. Client hello - This is the initial message to the server where the client sends the current version of TLS they are using followed by a list of cypher suites they support and a random number. The random number prevents replay attacks
2. The server will then send a hello message back. This will assert which cypher suite and TLS version to use if there are multiple options. Another random number will be sent.
3. The Server will now send a message containing a certificate. This is the method through which a server can prove their identity to the client. They will also then send the server key exchange message, if using Diffie-Hellman(see below) this is where the server would send the variables $p$ and $g$ followed by the calculated value $g^b \bmod p$ .
4. The client will then send back their part of the Diffie-Hellman key.
5. The client will then finish by sending a 'done' message. This will be encrypted with all the specifications established, the server after receiving this message will confirm everything is working by sending an encrypted 'done message' back.![mTLS](mTLS/mTLS)
## How mTLS differs
mTLS or mutual transport layer security follows exactly the same procedure as above but both members have to certify their identity. In the system detailed above, this means that after the server presents a certificate the client will also be required to provide a certificate. In certain scenarios both parties want to be confident they are dealing with the system that they claim to be it makes sense to use mTLS. In simple web applications the server side will have systems to verify clients like passwords or tokens that sit within the application but if you want safety from password theft or impersonation you could use mTLS. So it makes sense in scenarios where two big businesses are communicating.

It can also be implemented within an organisation to add security to the network. It adds extra roadblocks to intruders if they were able to gain access to the internal network. The organisation could have a self-signed certificate it issues to sub-systems restricting who can  mock authentication from outside the organisation - even with stolen passwords, tokens hackers might be unable to embed themselves deeply into a system.

## X.509 SSL Certificates 
### Certificates 
SSL Certificates provide a system for servers to prove their identity - a crucial part of web security. Certificates take advantage of public/private key encryption. A certificate is signed with a authority's private key which means when decrypted with the public key pair a viewer can be sure the certificate was signed by that authority and not an impostor. In the diagram above, the server hands a certificate over which will be signed by an authority that the client trusts, for instance a large reputable company. In practice this isn't the case and it is simply signed by another company who has a certificate from another source and this chains continues until the client is satisfied they have found a company they trust. Root CA's are certificates which are self signed by governing bodies or large tech firms like Apple,Microsoft. They typically use some other physical method to ensure certificate trustworthiness like secure physical distribution.
### X.509
X.509 is a standard used when creating and signing certificates. There are two types, end-entity and CA, which relate to certificates simply for authenticating a website and certificates which can then be used to sign more certificates. 

X.509 also has a system for distributing certificates deemed untrustworthy called the certificate revocation list. There is an algorithm which X.509 implements for checking certificates which is fairly simple. Certificate signings can be modelled as a tree where we start at a node(the server) and we simply climb the tree until we find a node that we are satisfied with.

## Encryption
There are various encryption methods used that take advantage of public/private key encryption. A famous method is the <b>Diffie-Hellman</b> key exchange. 
1. Alice and Bob publicly agree to use a given modulus p and base g. 
2. Alice and Bob then both choose a secret number each, a and b.
3. They perform the following calculation with their secret numbers a and b
4. $$A = g^{a} \bmod p <-> B = g^b \bmod p$$
 
5. Alice and Bob send these calculated values to each other which in essence make them both public.
6. $$s = B^{a}\bmod p = A^b\bmod p$$
7. Alice and Bob both raise the other persons computed public value to the power of their secret number then modding with p again. This will result in a shared number because of the nature of raising powers - they keep this number secret and use it as the key to encryption.
8. $$B^{a} \bmod p = A^b \bmod p = g^{ab} \bmod p$$

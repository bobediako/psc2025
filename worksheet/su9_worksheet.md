## ProLUG Security Engineering

# Unit 9 Worksheet

*Instructions*
*Fill out this sheet as you progress through the lab and discussions. Hold your worksheets until the end to turn them in as a final submission packet.*

---

## Discussion Questions:

### Unit 9 Discussion Post 1:
https://discord.com/channels/611027490848374811/1377483939706310736/1377726789794926611

Security
Unit 9 Discussion Post 1:
Read the Security Services section, pages 22-23 of https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r5.pdf and answer the following questions.

### How do these topics align with what you already know about system security?,

- confidentiality = the use of encryption to render information unintelligible except by an authorized entity

- data integrity = the goal of ensuring that data has not been modified by in unauthorized manner

- authentication = 3 types of authentication exist
 - - 1. identity = used to gain access to a service
 - - 2. integrity = used to verify that data has not been modified
- - 3. source = used to verify who created or sent a message

### Were any of the terms or concepts new to you?

- The separation of 3 types of authentication & their specific terms.


### Unit 9 Discussion Post 2: 

https://discord.com/channels/611027490848374811/1377484046757662801/1377733934670151811


Security

Unit 9 Discussion Post 2: Review the TLS Overview section, pages 4-7 of https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-52r2.pdf and answer the following questions


### 1. What are the three subprotocols of TLS?
- the handshake
> used to negotiaite the session protocols
> a series of messages are exchanged between the client and the server

- the change cipher spec
> used to notify the other party of an error condition

- the alert protocols
> used to change the cryptoographic parameters of a session
> also used to convey information about the session such as errors and warnings


### 2. How does TLS apply
a.    Confidentiality
> applied via the negotiated algorithm for the cipher suite and encryption keys


b.    Integrity
> applied via a keyed MAC algorithm during negotiations of the cipher suite between client and server


c.    Authentication
> applied by the client, using the server's public key certificate, during the initial handshake


d.    Anti-replay
> the envelope of each message contains a sequence number that increases with each message


## Definitions/Terminology

- TLS
	> Transport Layer Secuirty
	> a cryptographic protocol designed to provide communications security over a network.
	> the most common use case is in securing HTTPS
	> client - server applications use TLS to prevent eavesdropping and prevent tampering with data in transit
	> TLS is the successor to SSL and was developed to address vulnerabilities found in SSL.  TLS 1 could be thought of as SSL 4.
	``` Differences between TLS & SSL
		1. TLS uses stronger encryption (TLS -> HMAC vs SSL -> MAC)
		2. TLS handshakes use fewer steps
		3. TLS uses encrypted alert messages and cand end a session


- Symmetric Keys
	> This uses the same key to encrypt and decrypt data
	> This means the key must be shared with both parties and is possibly exposed during initial transit
	> Therefore symmetric keys are used for data at rest ... meaning data that stays in one place like on your hard disk and is not transmitted anywhere else

- Asymmetric Keys
	> This design uses two keys : one public and one private.
	> The public key is shared with everyone and used to encrypt data so that only the recipient that has access to the private key can read it.
	> The private key is kept secret by the person or enity that wants to receive data securely with others being able to read it.
	> a simple analogy is that the public key is really just a box and padlock... the sender puts items in the box and sues the padlock to lock it.. only the recipient can unlock it....

- Non-Repudiation
	> Non-repudiation in cryptography is ensuring that a source cannot deny the authenticity or integrity of a message or transaction they have sent or received.
	> 

- Anti-Replay
    > the anti-replay protocol provides IP packet-level security by making it impossible for a hacker to intercept message packets and insert changed packets into the data stream between a source and destination.

- Plaintext
    > basic characters that has not been changed or obsfucated.
    > unencrypted readable version of text.

- Cyphertext
    > this is plaintext that has been transformed into a state that it cannot be read by humans without a tool to decrypt.

- Fingerprints
    > this is a hash that is unique to a a certificate and an be used to confirm it was valid

- Passphrase (in key generation)
    > a passphrase is a longer version of a password ... to be more secure.








## Notes During Lecture/Class:

Links:
- https://www.sans.org/information-security-policy/
- https://www.sans.org/blog/the-ultimate-list-of-sans-cheat-sheets/
- https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r5.pdf
- https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-52r2.pdf

Terms:

Useful tools:
- STIG Viewer 2.18
- Ansible
- Killercoda


Lab and Assignment

Unit9-Certificates and keys - To be completed outside of lecture time.


Digging Deeper

1. Finish reading about TLS in the publication and think about where you might apply it.


Reflection Questions

1. What were newer topics to you, or alternatively what was a new application of
   something you already had heard about?
2. What questions do you still have about this week?
3. How are you going to use what you’ve learned in your current role?


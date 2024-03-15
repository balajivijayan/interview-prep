# SSL/TLS Handshake

## Introduction

SSL/TLS Handshake happens when a client establishes the connection with the server. The handshake is responsible for Privacy, integrity, and identification of the communication between the client and the server.

The words SSL and TLS are used interchangeably. SSL is the older spec and TLS is the recent. SSL v3 is TLS v1.1. The current spec version is TLS 1.3

Before learning about SSL/TLS Handshake, it's important to understand the below terminologies.

- Client and Server
- Cipher Suites
- Certificates
    - X.509
- Certificate Authority
    - DV, OV and EV
- Certificate Chaining
- Public and Private Key
- Hashing & HMAC
- Keystore
- Truststore
- Mutual TLS

### Client and Server
The client is the system or machine which initiates the connection. Ex, Browser/Postman/Client App.

The server is the system or machine which servers the response. Ex, NginX, Tomcat etc

The communication between the client and server is in network packets. Multiple records can be fit in a packet and sometimes multiple packets are required to carry a single record

### Cipher Suites

A cipher suite is a combination of the belwo four cryptography methods. These methods are agreed upon by the client and the server as the transport protocol.

- Key Exchange Protocol - To generate necessary keys
- Authentication - Verify Server's identity
- Symmetric Encryption - Confidentiality for Bulk data transfer
- Hashing Algorithm - Used with MAC for data integrity

Ex - TLS_ECDHE_ECDSSA_WITH_AES_256_GCM_SHA384. 
- ECDHE - Key Exchange
- ECDSA - Authentication
- AES_256_GCM - Symmetric Encryption
- SHA384 - Hashing

![Alt text](image-1.png)

### Certificates

TODO

### Hashing and MAC
Hashing is used to validate the data integrity. A hash function is used to convert the input message to a digest. Once the input message is hashed, the digest can't be reversed to get the input message. 

- MAC, Message Authentication Code is a hashing technique which also take a shared key between the sender and receiver and uses it along with the message to generate the digest. This helps to validate that only the known sender with the key has created the digest

![Alt text](image-2.png)



## References
[How https works](https://howhttps.works/episodes/)

[TLS Handshake](https://www.reddit.com/r/cybersecurity/comments/1126lt1/the_tls_handshake_everything_that_happens_to_get/)

[TLSv1.2 Flow Diagram](https://tls12.xargs.org/#server-certificate)


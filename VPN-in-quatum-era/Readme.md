# Virtual Private Networks in the Quantum Era: A Security in Depth Approach

Conventional asymmetric cryptography is threatened by the ongoing development of quantum computers. A
mandatory countermeasure in the context of virtual private networks (VPNs) is to use post-quantum cryptography (PQC) as a drop-in replacement for the authenticated key exchange in the Internet Key Exchange (IKE)
protocol.

## Conclusion

The development of quantum computers threatens current VPN cryptographic methods. Post-quantum cryptography (PQC) is a mandatory countermeasure, but it is still under scrutiny. The article proposes combining PQC with quantum key distribution (QKD) and multipath key reinforcement (MKR) for enhanced security. The proposed solution includes a lightweight proxy for IKE packets, using all available symmetric keys to create a cryptographic tunnel. This ensures that even if PQC is compromised, the VPN remains secure.


## Key Points

- **Quantum Threat**: Quantum computers pose a significant threat to current asymmetric cryptography used in VPNs.

- **PQC Importance**: Post-quantum cryptography is essential to replace classical algorithms like RSA and ECDSA.

- **Orthogonal Methods**: Combining PQC with QKD and MKR offers a stronger defense against quantum attacks.

- **Lightweight Proxy**: A lightweight proxy can transparently tunnel IKE packets using combined symmetric keys.

- **Security in Depth**: The proxy approach ensures security in depth, protecting VPN traffic even if PQC is compromised.

- **Entity Authentication**: Secure identification of VPN gateways is crucial for quantum-resistant VPNs.

- **Confidentiality**: Client traffic confidentiality must be guaranteed end-to-end, even with intermediate gateways.

- **Data Integrity**: Protecting data integrity and preventing replay attacks are vital for secure VPN communication.

- **Key Synchronization**: A protocol for synchronizing symmetric keys between proxies is necessary for consistent security.

- **Persistent Key Storage**: Secure storage of symmetric keys and recovery methods are essential for maintaining VPN security.

## Summary

1. **Quantum Threat**: Quantum computers threaten current VPN cryptographic methods, necessitating new approaches to ensure security.
   
2. **Post-Quantum Cryptography (PQC)**: Implementing PQC is mandatory to replace vulnerable classical algorithms in VPNs.

3. **Combining Security Methods**: Using PQC along with QKD and MKR provides a robust defense against potential quantum attacks.

4. **Lightweight Proxy Concept**: A proposed lightweight proxy can tunnel IKE packets using all available symmetric keys, enhancing security.

5. **Security in Depth**: This proxy method offers layered security, making it difficult for attackers to compromise VPN traffic.

6. **Entity Authentication**: Ensuring the secure identification of VPN gateways is critical for maintaining quantum-resistant VPNs.

7. **End-to-End Confidentiality**: VPNs must guarantee the confidentiality of client traffic throughout the entire communication path.

8. **Integrity and Replay Protection**: Detecting unauthorized data modifications and preventing replay attacks are essential for secure VPN operations.

9. **Key Synchronization Protocol**: Synchronizing symmetric keys between VPN proxies is necessary to maintain security consistency.

10. **Persistent Key Storage**: Securely storing symmetric keys and having reliable recovery methods are crucial for VPN security.


## Introduction

Asymmetric cryptography serves as a fundamental component of modern virtual private networks (VPNs). However, current cryptographic algorithms face significant threats from attackers equipped with sufficiently powerful quantum computers, referred to as quantum attackers in this paper.

#### Quantum Threat to Asymmetric Cryptography

Quantum computers have the potential to break classical algorithms like RSA and ECDSA, which are widely used in VPNs for secure communications. [**Shor's algorithm**](https://en.wikipedia.org/wiki/Shor%27s_algorithm), a well-known quantum algorithm, can efficiently solve problems that underpin the security of these classical cryptographic methods, making them vulnerable to quantum attacks.

#### Post-Quantum Cryptography (PQC)

Security agencies worldwide recommend adopting post-quantum cryptography (PQC) as a mandatory measure to counteract quantum threats. PQC algorithms are designed to be resistant to quantum attacks and can serve as drop-in replacements for classical algorithms within the Internet Key Exchange (IKE) protocol, specifically IKEv2, which is used for entity authentication and key exchange in VPNs.

#### Challenges with PQC

Despite its promise, the field of PQC is relatively young, and the confidence in PQC implementations is not yet on par with that of classical algorithms from the pre-quantum era. Recent cryptanalysis efforts have uncovered flaws in some PQC algorithms, including former finalists in the National Institute of Standards and Technology (NIST) standardization process.

#### Orthogonal Methods for Quantum-Resistant Key Exchange

Given the uncertainties surrounding PQC, this paper explores additional methods to secure VPNs against quantum attackers. These methods include:

- **Quantum Key Distribution (QKD)**: Utilizes the principles of quantum mechanics to establish secure keys between parties.
- **Multipath Key Reinforcement (MKR)**: Enhances security by distributing key shares across multiple paths in the network.

#### Combined Approach for Maximum Security

Each of these methods has its own limitations when used in isolation. Therefore, the paper advocates for a combined approach that leverages all available sources of symmetric key material to protect traffic within a VPN. 
> The proposed solution involves a lightweight proxy that transparently tunnels IKE packets, securing them with a combination of symmetric keys, such as QKD and MKR keys, in addition to PQC.

#### Security in Depth

By combining various quantum-resistant key exchange methods, the proposed approach aims to provide security in depth. This means that even if one method is compromised, the overall security of the VPN remains intact due to the additional layers of protection.

---

### Key Points

- **Asymmetric Cryptography**: Fundamental for modern VPNs but threatened by quantum computers.
  
- **Quantum Threat**: Quantum attackers can break classical algorithms like RSA and ECDSA using quantum algorithms such as Shor's algorithm.

- **Post-Quantum Cryptography (PQC)**: Recommended by security agencies as a countermeasure, but still relatively immature and less trusted than classical algorithms.

- **Quantum Key Distribution (QKD)**: Uses quantum mechanics to securely establish keys between parties.

- **Multipath Key Reinforcement (MKR)**: Distributes key shares across multiple network paths to enhance security.

- **Combined Approach**: Advocates using all available symmetric key sources (QKD, MKR, PQC) to protect VPN traffic.

- **Lightweight Proxy**: Proposed solution involves a proxy that tunnels IKE packets, using a combination of symmetric keys for enhanced security.

- **Security in Depth**: Ensures that even if one security method is compromised, overall VPN security remains robust.
## Background: Virtual Private Networks

This section provides an overview of site-to-site VPN deployments and their underlying principles, focusing on the use of IPsec for secure communication over untrusted public networks.

#### Site-to-Site VPN Deployment

In a site-to-site VPN deployment, VPN gateways connect multiple trusted private networks through secure tunnels over an untrusted public network. Each site, such as a company office or public authority, has a VPN gateway that tunnels traffic securely from clients within the local private network to remote sites.

![Figure 1](image1.jpg)

*Figure 1: Example for a site-to-site VPN deployment: VPN gateways connect multiple (trusted) private networks via an untrusted public network by establishing secure tunnels.*

#### IPsec Protocol Family

The VPN operates at the network layer using the IPsec protocol family, which includes the Internet Key Exchange version 2 (IKEv2) for entity authentication and key exchange. IKEv2 is responsible for establishing security associations (SAs), which are then used by the Encapsulating Security Payload (ESP) to ensure the confidentiality and integrity of data within these tunnels.

#### Security Associations (SAs)

Established SAs form an overlay topology that dynamically determines the optimal routing and scalability of the VPN. This overlay topology can handle complex scenarios such as nested or load-balanced VPN gateways. Packets must still be protected end-to-end, even when some gateways cannot directly reach each other due to external firewalls or nested scenarios.

#### Overlay Topology and Routing

To implement highly scalable and robust VPNs, the topology control algorithm dynamically determines the overlay topology. This algorithm manages routing within the overlay, ensuring end-to-end protection by implementing nested SAs where necessary. The architecture of a VPN gateway includes a control plane for topology control and a data plane for routing client packets, as depicted in Figure 2 of the document.

![Figure 2](image2.jpg)

*Figure 2: Basic architecture of a VPN gateway: A topology control algorithm decides which SAs should be established and manages overlay routing. The IKE daemon is responsible for establishing SAs using an authenticated key exchange. The data plane uses the derived TEK to protect client packets using ESP and forwards them based on the routing decision. Dashed lines represent control flow, solid lines represent physical interfaces.*

#### Conclusion

The background section emphasizes the importance of a dynamic and secure overlay topology for the scalability and robustness of VPNs. It highlights the roles of IKEv2 for entity authentication and key exchange and ESP for ensuring data confidentiality and integrity. This foundation sets the stage for discussing the security enhancements needed to protect VPNs against quantum threats.

### Key Points

- **Site-to-Site VPN Deployment**: Connects multiple trusted private networks via secure tunnels over an untrusted public network.

- **IPsec Protocol Family**: Utilizes IKEv2 for entity authentication and key exchange, and ESP for data confidentiality and integrity.

- **Security Associations (SAs)**: Establishes an overlay topology that supports dynamic routing and scalability, protecting data end-to-end even in complex scenarios.

- **Overlay Topology Control**: A topology control algorithm dynamically determines the optimal routing and scalability of the VPN.

- **Nested SAs**: Ensures end-to-end protection by implementing nested SAs where necessary.

- **VPN Gateway Architecture**: Includes a control plane for topology control and a data plane for routing client packets.

- **Scalability and Robustness**: Emphasizes the importance of a dynamic and secure overlay topology for the scalability and robustness of VPNs.
## Objectives and Threat Model

This section defines the objectives for a quantum-resistant VPN and outlines the assumed threat model, considering the capabilities of quantum attackers.

#### Objectives

A quantum-resistant VPN has the following enhanced objectives compared to conventional VPNs:

1. **Entity Authentication**: Gateways must securely identify each other, and private IP address ranges must be securely linked to gateways.

2. **Confidentiality**: The confidentiality of client traffic must be guaranteed. This includes end-to-end cryptographic protection of traffic routed via intermediate gateways and ensuring forward secrecy, so the compromise of long-term keys does not affect the confidentiality of past security associations (SAs).

3. **Data Integrity and Replay Protection**: Unauthorized modification and replay of data packets must be detected and prevented.

4. **Cryptographic Agility**: The cryptographic mechanisms should be easily exchangeable to adapt to new developments in cryptanalysis.

5. **Non-Functional Properties**: Implementing quantum resistance should not degrade scalability, robustness to denial-of-service (DoS) attacks, graceful degradation in case of compromised devices, and implementation security.

#### Threat Model

The threat model assumes an exceptionally strong Dolev-Yao attacker with access to a sufficiently large quantum computer:

- **External Attacker Capabilities**: The attacker can break classical asymmetric cryptography on controlled links, store traffic now and decrypt it later after finding flaws in PQC algorithms, and potentially compromise a limited number of VPN gateways or other security-critical entities like QKD devices.

- **Assumptions about Attacker Limitations**: The attacker’s computational power and eavesdropping capabilities are limited, preventing them from breaking symmetric cryptographic primitives with adequate key sizes (≥ 256 bits) or eavesdropping on all links at all times.

The threat model acknowledges the possibility of an attacker storing traffic for future decryption attempts and considers the need to protect against such long-term threats by ensuring robust forward secrecy and cryptographic agility.

---

### Key Points

- **Entity Authentication**: Gateways must securely identify each other and link private IP addresses to gateways.
  
- **Confidentiality**: Ensures end-to-end protection of client traffic and maintains forward secrecy.

- **Data Integrity and Replay Protection**: Detects and prevents unauthorized modification and replay of data packets.

- **Cryptographic Agility**: Allows for easy exchange of cryptographic mechanisms to adapt to new threats.

- **Non-Functional Properties**: Maintains scalability, robustness to DoS attacks, graceful degradation, and implementation security while introducing quantum resistance.

- **Dolev-Yao Attacker Model**: Assumes a strong attacker with quantum computing capabilities, able to break classical cryptography and store traffic for future decryption.

- **Attacker Limitations**: The attacker's ability to break symmetric cryptographic primitives and eavesdrop on all links is limited by computational power and resources.

- **Protection Against Long-Term Threats**: Emphasizes the importance of forward secrecy and cryptographic agility to guard against future decryption attempts.
## Related Work

This section discusses the two-step process of implementing security services (entity authentication, confidentiality, and data integrity) in VPNs with IPsec, focusing on authenticated key exchange and authenticated encryption. The section then reviews various approaches to achieving quantum-resistant VPNs, including post-quantum cryptography (PQC) and symmetric key management methods.

#### Authenticated Key Exchange and Authenticated Encryption

1. **Authenticated Key Exchange**: Establishing a new Security Association (SA) between two VPN gateways involves mutual authentication and the derivation of a new symmetric Traffic Encryption Key (TEK).

2. **Authenticated Encryption**: Using the derived TEK, gateways encrypt all client packets and add authentication tags to protect their integrity.

Since symmetric cryptography with sufficiently large keys (≥ 256 bits) remains secure in the quantum era, only the authenticated key exchange needs to be adapted to ensure quantum resistance. This can be achieved using PQC or symmetric cryptography.

#### Post-Quantum Cryptography (PQC)

PQC algorithms can serve as drop-in replacements for classical asymmetric cryptographic methods like RSA, ECDSA, and Diffie-Hellman key exchange. Several PQC algorithms are being standardized by NIST, including three signature schemes and one key encapsulation mechanism (KEM).

However, PQC has its drawbacks:

- **Efficiency**: PQC algorithms are generally less efficient than classical elliptic curve cryptography, leading to higher computational overhead and larger key and signature sizes.
- **Maturity**: The cryptanalysis of PQC algorithms is not as mature, as evidenced by recent discoveries of flaws in NIST's former finalists.

As a result, hybrid modes that combine classical and PQC algorithms are recommended for authenticated key exchanges.

#### Symmetric Key Management

An alternative approach to achieve quantum resistance involves using symmetric key management methods, such as pre-shared keys (PSKs), quantum key distribution (QKD), and multipath key reinforcement (MKR).

1. **Pre-Shared Keys (PSKs)**: Pairwise PSKs can be deployed between gateways, but managing and updating PSKs is cumbersome. Static group keys provide convenience but are insecure if any gateway is compromised.

2. **Quantum Key Distribution (QKD)**: QKD uses qubits for key exchange, providing security based on quantum mechanics principles. However, QKD is limited by the need for direct optical connections and high operational costs. Trusted nodes can relay QKD keys over longer distances but do not provide end-to-end security.

3. **Multipath Key Reinforcement (MKR)**: MKR splits keys into shares and transmits them over different paths, increasing the security if attackers cannot eavesdrop on all paths. MKR can be combined with QKD for enhanced security.

#### Using PSKs in IPsec

Externally exchanged keys, such as those derived from QKD and MKR, can be considered PSKs in IKE. Extensions to IKEv2 allow the incorporation of PSKs into TEKs, protecting IKE packets and reducing metadata leakage. However, this approach does not address potential flaws in IKE implementations.

Some proposals suggest using only QKD keys, completely replacing IKE, but this is currently ruled out by various national security agencies.

#### Lessons Learned

PQC is a mandatory approach to secure VPNs against quantum threats, and it should be combined with pairwise PSKs for added protection. Each method of exchanging PSKs has limitations, so a flexible mechanism to combine all symmetric keys is desirable. Extending IKE to handle this combination increases complexity, so a separate lightweight component for managing symmetric keys is recommended.

---

### Key Points

- **Authenticated Key Exchange**: Establishes mutual authentication and derives new symmetric keys between VPN gateways.
  
- **Authenticated Encryption**: Protects client packets using the derived symmetric keys and authentication tags.

- **Post-Quantum Cryptography (PQC)**: Serves as a replacement for classical cryptography but has efficiency and maturity issues.

- **Hybrid Modes**: Combine classical and PQC algorithms to enhance security.

- **Symmetric Key Management**: Includes methods like PSKs, QKD, and MKR for achieving quantum-resistant VPNs.

- **Pre-Shared Keys (PSKs)**: Convenient but challenging to manage and insecure if compromised.

- **Quantum Key Distribution (QKD)**: Secure but limited by optical connection requirements and high costs.

- **Multipath Key Reinforcement (MKR)**: Enhances security by transmitting key shares over multiple paths.

- **Incorporating PSKs in IPsec**: Extensions to IKEv2 allow using PSKs to protect IKE packets, reducing metadata leakage.

- **Flexible Mechanism**: A separate lightweight component for managing symmetric keys is recommended to avoid increasing IKE complexity.

## IKE Proxy Design

This section presents a lightweight proxy design that transparently tunnels and cryptographically protects IKE packets. The design aims to combine various sources of symmetric key material to reinforce security and ensure quantum resistance.

#### Overview

The envisioned architecture of a VPN gateway with an IKE proxy includes the following functions:

1. **Packet Interception**: The proxy must intercept all IKE packets to protect them mandatorily. This can be achieved using firewall rules on Linux-based systems to direct packets to the proxy, implemented as a user-space process.

2. **Proxy Header**: To implement a cryptographic tunnel, the proxy introduces a header before the IKE payload, similar to the transport mode in IPsec. This header includes fields for algorithm ID, nonce, authentication tag, temporary key ID, and encrypted message type.

3. **Dynamic Address Mapping**: The proxy needs to map key material to the corresponding remote proxy dynamically. This requires a minimal greeting protocol to associate transport addresses (IP address and port) with static peer IDs, which must adapt to changes like network address translation (NAT).

4. **Key Synchronization**: A protocol is required to synchronize symmetric keys between proxies. This protocol runs independently of the IKE traffic and may be tunneled via the VPN, ensuring the proxies agree on a set of keys to combine into a master key.

![Figure 4](image4.jpg)


*Figure 4: Tunneling IKE packets is implemented similar to the transport mode in IPsec. The original IP and UDP headers are updated (lengths and checksums) and a proxy header is introduced before the IKE payload. The latter is protected by the proxy using authenticated encryption.*

5. **Secure and Persistent Key Storage**: VPN gateways must securely store keys to withstand reboots. The design includes a secure persistent storage solution to ensure the availability of key material after power loss.

6. **TEK Reinforcement**: The derived TEK should be reinforced by combining it with the master key of the proxy to protect against potential flaws in PQC algorithms or random number generators.

![Figure 1](image3.jpg)

*Figure 3: Envisioned architecture of a VPN gateway with an IKE proxy, which interfaces to various sources of symmetric key material. The master key is used to implement a quantum-resistant tunnel for IKE.*

#### Proxy Header

The proxy header, introduced between the UDP header and the IKE payload, includes the following fields:

1. **Algorithm ID**: Identifies the applied authenticated encryption scheme.
2. **Nonce**: Used by the authenticated encryption scheme.
3. **Authentication Tag**: Generated by the authenticated encryption scheme.
4. **Temporary Key ID**: Indicates the master key in use.
5. **Encrypted Message Type**: Specifies the message type, relevant only after decryption.

The header ensures that the proxy can tunnel IKE packets securely without breaking features like NAT traversal.

#### Greeting Protocol

To map an unknown IP address and port to a peer ID, the proxy uses a greeting protocol:

1. The initiator proxy sends its peer ID encrypted with a static group key.
2. The responder proxy replies with its peer ID, encrypted with either the static group key or an existing pairwise master key.
3. This exchange allows the initiator to map the temporary key ID in the proxy header to the responder's peer ID.

![Figure 5](image5.jpg)

*Figure 5: Greeting protocol to query the peer ID corresponding to an unknown IP address and port.*

#### Key Synchronization Protocol

The key synchronization protocol ensures proxies agree on the master key by combining available symmetric keys over time:

1. Each symmetric key has a unique key ID.
2. Two proxies maintain separate master keys for each direction to avoid race conditions.
3. Synchronization requests include key IDs and proof of knowledge, ensuring keys are in sync.

![Figure 1](image6.jpg)

*Figure 6: Derivation of master keys by combining previous master keys and new symmetric keys using a key derivation function (KDF).*

#### Secure and Persistent Key Storage

For secure key storage:

1. Use a trust anchor (e.g., a smartcard) to protect recovery keys, which are cryptographic hashes of shared keys.
2. Recovery involves exchanging nonces and deriving keys securely using the trust anchor.
3. This approach ensures keys are recoverable after a reboot without posing a performance bottleneck during normal operations.

![Figure 1](image7.jpg)

*Figure 7: Synchronization of a key between proxies in both directions using encrypted messages and proof of knowledge.*

#### TEK Reinforcement

The TEK derived by IKE is implicitly protected by the cryptographic tunnel. For additional security, the TEK is combined with the master key using a key derivation function (KDF). This protects against fundamental flaws in PQC algorithms or random number generators.

---

### Key Points

- **Packet Interception**: Proxy intercepts all IKE packets for mandatory protection.
  
- **Proxy Header**: Introduces a header before the IKE payload to implement a cryptographic tunnel.

- **Dynamic Address Mapping**: Maps key material to remote proxies dynamically, adapting to network changes.

- **Key Synchronization**: Protocol ensures proxies agree on a master key by combining available symmetric keys.

- **Secure Key Storage**: Ensures key availability after reboots with secure persistent storage using a trust anchor.

- **TEK Reinforcement**: Combines the TEK with the master key for additional security against PQC flaws.

- **Greeting Protocol**: Associates transport addresses with peer IDs securely.

- **Symmetric Key Material**: Incorporates various symmetric key sources like QKD, MKR, and PSKs.

- **Cryptographic Tunnel**: Provides security in depth by protecting IKE packets with multiple layers of encryption.

- **Scalability**: Proxy design supports numerous VPN gateways without performance degradation.

## Discussion

This section evaluates the IKE proxy approach in relation to the objectives defined in Section 3, discussing its impact on various aspects such as entity authentication, confidentiality, data integrity, cryptographic agility, scalability, robustness, graceful degradation, and implementation security.

#### Entity Authentication

If the IKE daemon's combination of post-quantum cryptography (PQC) and classical digital signature schemes is secure, entity authentication will be quantum-resistant. The design does not modify the IKE protocol, ensuring that once the proxy synchronizes at least one symmetric key unknown to the attacker, all following master keys provide authenticity. This prevents external attackers from exploiting potential flaws in the PQC signature scheme to impersonate gateways.

#### Confidentiality

The confidentiality of client traffic is protected by the TEK derived by IKE, which uses a hybrid key exchange combining PQC and classical methods. Attackers might store IKE exchanges and client traffic now, hoping to decrypt them later if a flaw is found in the PQC algorithm. However, if the IKE and the final TEK are protected by a master key unknown to the attacker, the cost of such "store now, decrypt later" attacks increases significantly.

**Protection Layers**:
1. **Static Group Key**: Forces attackers to compromise at least one gateway.
2. **QKD Keys**: Secure if adjacent QKD devices are uncompromised and the classical channel is authenticated.
3. **Pairwise PSKs**: Requires compromising one of the adjacent gateways.
4. **MKR Keys**: Forces attackers to decrypt multiple SAs and paths, especially if some paths are protected by QKD.

#### Data Integrity and Replay Protection

Unauthorized modification or replay of client and key synchronization packets is detected if the attackers do not know the TEK. The TEK is protected by PQC exchanges within the IKE protocol and further reinforced by the proxy’s master key, ensuring robust data integrity and replay protection.

#### Cryptographic Agility

The IKE proxy can synchronize symmetric key material from various sources and easily exchange the symmetric authenticated encryption scheme used to protect IKE packets due to the algorithm ID in the proxy header. This flexibility allows the proxy to adapt to changes in cryptographic requirements without modifying IKE or its configuration.

#### Scalability

The IKE proxy, relying solely on symmetric cryptography, poses no performance bottleneck for establishing or rekeying thousands of SAs. Memory and disk consumption scale linearly with the number of gateways in the VPN. Old key material and master keys can be deleted after successful key synchronization, ensuring efficient resource use.

#### Robustness to DoS Attacks

The IKE proxy is resilient to denial-of-service (DoS) attacks due to its use of symmetric cryptography and minimal storage requirements per remote gateway. Simple request/reply mechanisms in the greeting and key synchronization protocols prevent amplification attacks. The protocol only synchronizes key material identical at both ends, ensuring attackers cannot force master keys out of sync.

#### Graceful Degradation

Compromising the IKE proxy does not introduce vulnerabilities beyond those possible by compromising other gateway components, such as the IKE daemon. The proxy’s design ensures no new attack vectors are introduced, maintaining overall system integrity.

#### Implementation Security

The proxy’s lightweight design and exclusive use of symmetric cryptography allow for a small implementation, adding minimal complexity to the trusted computing base. A small implementation facilitates formal security analysis. The master keys’ protection reduces the attack surface via public channels compared to an IKE daemon operating without a proxy.

---

### Key Points

- **Entity Authentication**: Ensures quantum-resistant authentication without modifying the IKE protocol.

- **Confidentiality**: Protects client traffic with layered security, making decryption attacks more expensive and difficult.

- **Data Integrity and Replay Protection**: Detects unauthorized modifications and replays if the TEK is secure.

- **Cryptographic Agility**: Allows easy exchange of cryptographic mechanisms, adapting to new threats.

- **Scalability**: Efficiently handles thousands of SAs without performance bottlenecks, scaling resources as needed.

- **Robustness to DoS Attacks**: Resilient to DoS attacks due to minimal storage requirements and symmetric cryptography.

- **Graceful Degradation**: Maintains overall system integrity even if the IKE proxy is compromised.

- **Implementation Security**: Small, focused implementation facilitates formal security analysis and reduces the attack surface.

- **Layered Security**: Combines multiple sources of symmetric key material for enhanced protection.

- **Dynamic Key Management**: Supports dynamic address mapping and key synchronization to adapt to network changes.
## Conclusion
In conclusion, implementing a transparent tunnel for Internet Key Exchange (IKE) via the proposed IKE proxy design enhances the security of Virtual Private Networks (VPNs) against quantum attackers without compromising the functional, non-functional, or security properties of existing VPNs.

#### Additional Line of Defense

The IKE proxy provides an additional line of defense by combining various symmetric key sources, such as pairwise pre-shared keys (PSKs), Quantum Key Distribution (QKD), and Multipath Key Reinforcement (MKR). This layered security approach ensures that even if one method is compromised, the overall security of the VPN remains robust.

#### Flexible and Scalable Design

The design of the IKE proxy is flexible and easy to integrate into a variety of VPN infrastructures. It can protect a hypothetical successor of the IKE protocol and can be implemented without extensive modifications to existing systems. The proxy's lightweight nature ensures scalability and efficient performance, handling numerous VPN gateways without degradation.

#### Future Work

Future work will focus on formal analyses of the correctness and security of the proposed protocols and their implementation. Additionally, the authors plan to explore convenient methods for distributing pairwise PSKs, such as using personnel laptops as key carriers during business trips between VPN sites. Another area of interest is studying the implications of MKR and the potential to tunnel the classical channel of QKD devices via co-located VPN gateways to further reduce the overall complexity and attack surface of VPNs.

#### Acknowledgments

This research is funded by the Digitalization and Technology Research Center of the Bundeswehr (dtec.bw) as part of the MuQuaNet project, supported by the European Union - NextGenerationEU.

---

### Key Points

- **Additional Line of Defense**: Combines various symmetric key sources to ensure robust VPN security.
  
- **Flexible Design**: Easily integrates into existing and future VPN infrastructures without extensive modifications.

- **Scalability**: Handles numerous VPN gateways efficiently, ensuring performance and security.

- **Future Work**: Focuses on formal security analyses and exploring methods for convenient key distribution and further reducing VPN complexity.

- **Funding and Support**: Research funded by dtec.bw and supported by the European Union - NextGenerationEU.

## Overall Conclusion and Key Points

The paper "Virtual Private Networks in the Quantum Era: A Security in Depth Approach" addresses the pressing need to secure VPNs against the emerging threats posed by quantum computing. It emphasizes the limitations of current cryptographic methods and proposes a comprehensive, layered security approach.

#### Key Highlights:

1. **Quantum Threat**:
   - Quantum computers pose a significant threat to classical asymmetric cryptography, such as RSA and ECDSA, which are widely used in VPNs.

2. **Post-Quantum Cryptography (PQC)**:
   - PQC is essential for securing VPNs against quantum attacks, though it is still in the early stages of development and less efficient than classical methods.

3. **Combined Approach**:
   - The paper advocates for a combined approach using PQC, Quantum Key Distribution (QKD), and Multipath Key Reinforcement (MKR) to enhance security. This layered strategy ensures that if one method fails, others provide a backup, thus maintaining overall security.

4. **IKE Proxy Design**:
   - A proposed lightweight proxy intercepts and secures IKE packets by combining various sources of symmetric key material. This proxy design ensures dynamic key synchronization, secure key storage, and enhanced security for IKE packets.

5. **Scalability and Robustness**:
   - The proxy design supports numerous VPN gateways without performance degradation and resists denial-of-service (DoS) attacks due to its efficient use of symmetric cryptography.

6. **Cryptographic Agility**:
   - The proxy allows for easy exchange of cryptographic mechanisms to adapt to new threats, ensuring long-term security adaptability.

7. **Dynamic Key Management**:
   - The proxy supports dynamic address mapping and key synchronization, adapting to network changes and maintaining secure communication channels.

8. **Implementation Security**:
   - The lightweight design minimizes the attack surface and facilitates formal security analysis, contributing to robust implementation security.

9. **Future Work**:
   - Future research will focus on formal security analyses, efficient key distribution methods, and exploring ways to further reduce VPN complexity while enhancing security.

### Conclusion

The paper concludes that a multi-layered security approach, combining PQC, QKD, and MKR, significantly enhances VPN security against quantum threats. The proposed IKE proxy design ensures robust, scalable, and adaptable security without compromising existing functionalities. Continued research and development are essential to further solidify these security measures and address any emerging vulnerabilities.

# Virtual Private Networks in the Quantum Era: A Security in Depth Approach

Conventional asymmetric cryptography is threatened by the ongoing development of quantum computers. A
mandatory countermeasure in the context of virtual private networks (VPNs) is to use post-quantum cryptography (PQC) as a drop-in replacement for the authenticated key exchange in the Internet Key Exchange (IKE)
protocol.

## Conclusion

The development of quantum computers threatens current VPN cryptographic methods. Post-quantum cryptography (PQC) is a mandatory countermeasure, but it is still under scrutiny. The article proposes combining PQC with quantum key distribution (QKD) and multipath key reinforcement (MKR) for enhanced security. The proposed solution includes a lightweight proxy for IKE packets, using all available symmetric keys to create a cryptographic tunnel. This ensures that even if PQC is compromised, the VPN remains secure.


## Key Points

📌 **Quantum Threat**: Quantum computers pose a significant threat to current asymmetric cryptography used in VPNs.

📌 **PQC Importance**: Post-quantum cryptography is essential to replace classical algorithms like RSA and ECDSA.

📌 **Orthogonal Methods**: Combining PQC with QKD and MKR offers a stronger defense against quantum attacks.

📌 **Lightweight Proxy**: A lightweight proxy can transparently tunnel IKE packets using combined symmetric keys.

📌 **Security in Depth**: The proxy approach ensures security in depth, protecting VPN traffic even if PQC is compromised.

📌 **Entity Authentication**: Secure identification of VPN gateways is crucial for quantum-resistant VPNs.

📌 **Confidentiality**: Client traffic confidentiality must be guaranteed end-to-end, even with intermediate gateways.

📌 **Data Integrity**: Protecting data integrity and preventing replay attacks are vital for secure VPN communication.

📌 **Key Synchronization**: A protocol for synchronizing symmetric keys between proxies is necessary for consistent security.

📌 **Persistent Key Storage**: Secure storage of symmetric keys and recovery methods are essential for maintaining VPN security.

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



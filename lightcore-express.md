# Lightcore-Express VPN Protocol Investigation

This file is dedicated to the investigation and analysis of the Lightcore-Express VPN protocol. The aim is to understand its workings, security implications, and potential improvements.

## Design
**Lightweight and Efficient:** Lightway is designed to be a lightweight and efficient VPN protocol. This means minimal code complexity and resource usage, which translates to faster performance and reduced battery consumption on devices.

**Security:** The design emphasizes security, employing modern cryptographic techniques to ensure data confidentiality and integrity. It aims to provide a secure communication channel that is resistant to various types of attacks.

**Simplicity:** The protocol is designed to be simple, both in terms of configuration and deployment. This simplicity helps in maintaining the code and ensuring fewer bugs and vulnerabilities.

**Cross-Platform Compatibility:** Lightway aims to be compatible across different platforms, including various operating systems and devices. This broad compatibility ensures that users can benefit from the protocol regardless of their device or operating system.

**Reliability and Stability:** The design prioritizes reliable and stable connections. It includes mechanisms to handle network changes and disruptions seamlessly, ensuring continuous connectivity for users.

## Implementation

**Core Protocol Logic:** This is implemented in C for performance reasons. The core logic handles the establishment, maintenance, and teardown of VPN connections.

**Cryptographic Operations:** Implementations of cryptographic algorithms used in the protocol, ensuring secure data transmission. This includes encryption, decryption, and key exchange mechanisms.

**Platform Abstractions:** Abstraction layers that handle platform-specific operations, such as network I/O, threading, and system calls. This ensures the core protocol logic remains platform-independent.


## Core Protocol Logic

The core protocol logic of Lightway typically includes the following components:

**Handshake:** Establishes a secure connection between the client and the server.

**Encryption/Decryption:** Handles the encryption and decryption of data packets.

**Data Transmission:** Manages the sending and receiving of data over the VPN tunnel.

**Connection Management:** Maintains the VPN connection, handles reconnections, and manages session states.

## Similarities with Other Protocols
**Handshake Mechanism:** Similar to protocols like WireGuard, Lightway uses a modern cryptographic handshake to establish a secure connection. WireGuard also uses a key pair and performs a Diffie-Hellman exchange to derive a shared secret.

**Encryption Methods:** Lightway's use of AES-256-GCM for encryption is similar to many protocols, including OpenVPN. AES-GCM is known for its performance and security, making it a common choice for VPN protocols.

**Simplicity and Performance:** Both Lightway and WireGuard emphasize simplicity and performance. They aim to be efficient and easy to implement, reducing the potential for vulnerabilities and ensuring better performance.

**Cross-Platform Compatibility:** Like OpenVPN and WireGuard, Lightway is designed to work across various platforms, ensuring users can benefit from the VPN on different devices and operating systems.


### Overall Mechanism of Handshaking in Lightway Core

The handshake mechanism in Lightway Core, implemented in the `conn.c` and related files, involves several steps to securely establish a connection between a client and a server. Here is an overview of the process:

1. **Initialization**:
   - Both client and server initialize their respective connection objects (`he_conn_t`) and SSL contexts (`he_ssl_ctx_t`).
   - They configure necessary parameters such as connection type, protocol version, and cryptographic settings.

2. **Client Connection Request**:
   - The client initiates the connection by calling `he_conn_client_connect`.
   - This function validates the client's configuration and prepares the client for the connection process.

3. **Server Connection Preparation**:
   - The server prepares to accept a connection by calling `he_conn_server_connect`.
   - This function validates the server's configuration, sets up necessary cryptographic parameters, and generates a session ID.

4. **Configuration**:
   - Both client and server use `he_internal_conn_configure` to apply configurations from the SSL context to the connection object.
   - This includes setting up callbacks, MTU sizes, and other connection-specific parameters.

5. **SSL Context Setup**:
   - A new WolfSSL object is created for each connection using `wolfSSL_new`.
   - The SSL context is configured according to the connection type (streaming or datagram) and other settings.

6. **Public Key Exchange**:
   - During the handshake, the client and server exchange public keys to establish a shared secret using the Elliptic Curve Diffie-Hellman (ECDH) key exchange method.
   - If post-quantum cryptography (PQC) is enabled, additional key shares may be exchanged.

7. **Session Establishment**:
   - The client and server negotiate the SSL/TLS session parameters using `wolfSSL_negotiate`.
   - This process involves verifying certificates (if applicable), agreeing on encryption algorithms, and deriving session keys.

8. **Authentication**:
   - After the SSL/TLS session is established, the client authenticates to the server using the configured authentication method (e.g., username/password, token, or buffer).
   - The server validates the authentication credentials and either accepts or rejects the connection.

9. **State Management**:
   - The connection state is managed throughout the handshake process, transitioning through states such as `CONNECTING`, `AUTHENTICATING`, and `ONLINE`.
   - State changes trigger callbacks to handle events like successful connection, authentication, and errors.

10. **Secure Communication**:
    - Once the handshake is complete and the connection is authenticated, the client and server can securely communicate using the established SSL/TLS session.
    - Data is encrypted and decrypted using the negotiated session keys, ensuring confidentiality and integrity.

11. **Error Handling and Recovery**:
    - The connection mechanism includes robust error handling to manage issues like invalid configurations, connection failures, and authentication errors.
    - Non-fatal errors may trigger retries or state changes to attempt recovery, while fatal errors result in connection termination.

This process ensures that both the client and server establish a secure, authenticated connection with minimal overhead, leveraging modern cryptographic techniques to maintain security and performance.

## `he_return_code`

1. **Navigate to `conn.h` File**:
   - Open the `conn.h` file located in the `src/he` directory of the repository.

2. **Search for `he_return_code` Definition**:
   - Look for the `typedef enum` keyword, which defines enumerations.

### Example of What You Might Find

Here is an example of what the `typedef enum he_return_code` might look like:

```c
typedef enum {
    HE_SUCCESS = 0,
    HE_ERR_NULL_POINTER,
    HE_ERR_MEMORY_ALLOCATION,
    HE_ERR_INVALID_ARGUMENT,
    HE_ERR_SSL_ERROR,
    HE_ERR_CONNECTION_FAILED,
    HE_ERR_AUTHENTICATION_FAILED,
    HE_ERR_INVALID_STATE,
    HE_ERR_TIMEOUT,
    // Add more error codes as needed
} he_return_code_t;
```

### Explanation of `he_return_code`

The `he_return_code` enumeration defines various return codes used throughout the Lightway Core codebase to indicate the result of function calls. Each value in the enumeration represents a specific type of result or error that can occur.

- **HE_SUCCESS**: Indicates that the function call was successful.
- **HE_ERR_NULL_POINTER**: Indicates that a null pointer was passed to a function where it is not allowed.
- **HE_ERR_MEMORY_ALLOCATION**: Indicates that memory allocation failed.
- **HE_ERR_INVALID_ARGUMENT**: Indicates that an invalid argument was provided to a function.
- **HE_ERR_SSL_ERROR**: Indicates that an error occurred within the SSL/TLS layer.
- **HE_ERR_CONNECTION_FAILED**: Indicates that establishing a connection failed.
- **HE_ERR_AUTHENTICATION_FAILED**: Indicates that authentication failed.
- **HE_ERR_INVALID_STATE**: Indicates that the function was called in an invalid state.
- **HE_ERR_TIMEOUT**: Indicates that an operation timed out.

### How to Use `he_return_code`

Functions throughout the Lightway Core codebase return `he_return_code_t` values to indicate success or various error conditions. Here is an example function using `he_return_code_t`:

```c
he_return_code_t he_conn_connect(he_conn_t *conn) {
    if (conn == NULL) {
        return HE_ERR_NULL_POINTER;
    }

    // Perform connection steps
    if (/* some condition */) {
        return HE_ERR_CONNECTION_FAILED;
    }

    return HE_SUCCESS;
}
```

### Conclusion

The `he_return_code_t` enumeration is a critical part of the error handling mechanism in the Lightway Core codebase. By defining specific error codes, it allows functions to return meaningful results that can be used to diagnose and handle errors appropriately.
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


### Overview of WolfSSL

WolfSSL is a lightweight, portable, and embedded SSL/TLS library designed for resource-constrained environments such as embedded systems, IoT devices, and real-time operating systems. It provides robust cryptographic functions and secure communication capabilities while maintaining a small footprint and high performance.

### Key Features of WolfSSL

1. **Lightweight and Portable**:
   - Optimized for size and speed, making it suitable for embedded systems and low-power devices.
   - Portable across various operating systems and platforms, including Linux, Windows, macOS, iOS, and Android.

2. **Secure Communication**:
   - Supports SSL/TLS protocols up to TLS 1.3, providing secure communication channels for data transmission.
   - Implements modern cryptographic algorithms such as AES, RSA, ECC, and post-quantum cryptography (PQC).

3. **Comprehensive Cryptography**:
   - Provides a wide range of cryptographic functions, including symmetric encryption, asymmetric encryption, hashing, and digital signatures.
   - Includes support for hardware cryptographic acceleration on various platforms.

4. **Flexibility and Configurability**:
   - Highly configurable, allowing users to enable or disable features as needed to optimize for size, speed, or security.
   - Supports custom memory management and various compiler options.

### How WolfSSL Works

WolfSSL operates by providing a library of functions that applications can use to establish secure communication channels. Here is an overview of how WolfSSL works:

#### 1. Initialization

Before using WolfSSL functions, the library must be initialized. This involves setting up the necessary contexts and configurations.

```c
#include <wolfssl/ssl.h>

// Initialize WolfSSL library
wolfSSL_Init();
```

#### 2. SSL Context Setup

An SSL context (`WOLFSSL_CTX`) holds configuration settings and information needed for SSL/TLS connections. Applications create and configure this context based on their requirements.

```c
// Create and configure SSL context
WOLFSSL_CTX* ctx = wolfSSL_CTX_new(wolfTLSv1_2_client_method());

// Set context options (e.g., cipher suites, certificates)
wolfSSL_CTX_set_options(ctx, WOLFSSL_OP_NO_SSLv3);
```

#### 3. SSL/TLS Session Establishment

To establish a secure session, applications create an SSL object (`WOLFSSL`) from the context. This object is used to manage the SSL/TLS session, including handshaking, encryption, and data transfer.

```c
// Create SSL object from context
WOLFSSL* ssl = wolfSSL_new(ctx);

// Associate SSL object with a socket (e.g., a TCP socket)
wolfSSL_set_fd(ssl, sockfd);
```

#### 4. Handshaking

The SSL/TLS handshake process involves exchanging cryptographic information between the client and server to establish a secure session. This includes certificate verification, key exchange, and session key derivation.

```c
// Perform SSL/TLS handshake
if (wolfSSL_connect(ssl) != SSL_SUCCESS) {
    // Handle handshake error
    int err = wolfSSL_get_error(ssl, 0);
    // Error handling logic
}
```

#### 5. Data Transfer

Once the handshake is complete, applications can securely send and receive data using the established SSL/TLS session.

```c
// Send data securely
const char* msg = "Hello, secure world!";
wolfSSL_write(ssl, msg, strlen(msg));

// Receive data securely
char buffer[256];
int bytes = wolfSSL_read(ssl, buffer, sizeof(buffer));
// Handle received data
```

#### 6. Connection Termination

After the data transfer is complete, the SSL/TLS session can be terminated, and the resources associated with the SSL object and context can be freed.

```c
// Terminate SSL/TLS session
wolfSSL_shutdown(ssl);

// Free SSL object
wolfSSL_free(ssl);

// Free SSL context
wolfSSL_CTX_free(ctx);

// Cleanup WolfSSL library
wolfSSL_Cleanup();
```

### Key Functions and Concepts in WolfSSL

1. **wolfSSL_Init()**:
   - Initializes the WolfSSL library. Must be called before any other WolfSSL functions.

2. **wolfSSL_CTX_new()**:
   - Creates a new SSL context with a specified method (e.g., client or server, SSL/TLS version).

3. **wolfSSL_new()**:
   - Creates a new SSL object for managing an SSL/TLS session.

4. **wolfSSL_set_fd()**:
   - Associates an SSL object with a file descriptor (e.g., a socket).

5. **wolfSSL_connect()**:
   - Performs the SSL/TLS handshake for a client.

6. **wolfSSL_accept()**:
   - Performs the SSL/TLS handshake for a server.

7. **wolfSSL_write()**:
   - Sends data securely over the SSL/TLS session.

8. **wolfSSL_read()**:
   - Receives data securely over the SSL/TLS session.

9. **wolfSSL_shutdown()**:
   - Terminates the SSL/TLS session.

10. **wolfSSL_free()**:
    - Frees the SSL object.

11. **wolfSSL_CTX_free()**:
    - Frees the SSL context.

### Conclusion

WolfSSL provides a comprehensive and efficient library for implementing SSL/TLS secure communication in a wide range of applications. Its lightweight design and configurability make it particularly well-suited for embedded systems and IoT devices. By leveraging WolfSSL, applications can ensure secure data transmission and robust cryptographic protection in resource-constrained environments.


Lightway Core uses WolfSSL for several reasons, leveraging its advantages over other SSL/TLS libraries and protocols. Here are the key benefits and reasons for choosing WolfSSL:

### Benefits of Using WolfSSL

1. **Lightweight and Performance Optimized**:
   - **Small Footprint**: WolfSSL is designed to be lightweight, making it suitable for resource-constrained environments such as embedded systems and IoT devices.
   - **High Performance**: Optimized for speed, WolfSSL can handle high-throughput and low-latency requirements efficiently.

2. **Strong Security**:
   - **Modern Cryptography**: Supports up-to-date cryptographic standards, including TLS 1.3, and various modern ciphers and protocols.
   - **Post-Quantum Cryptography**: Includes support for post-quantum cryptographic algorithms, providing future-proof security.
   - **Regular Updates and Audits**: WolfSSL is regularly updated and audited to ensure security and compliance with industry standards.

3. **Wide Platform Support**:
   - **Cross-Platform Compatibility**: WolfSSL is portable across many platforms, including Linux, Windows, macOS, iOS, and Android.
   - **Embedded Systems**: Specifically designed to run on embedded systems with limited resources.

4. **Extensive Features**:
   - **Protocol Support**: Supports SSL, TLS (up to 1.3), DTLS, and more.
   - **Hardware Acceleration**: Can leverage hardware cryptographic accelerators to enhance performance.
   - **Flexible Configuration**: Highly configurable to enable or disable features as needed.

5. **Ease of Integration**:
   - **Simple API**: Provides a simple and consistent API, making it easier to integrate into applications.
   - **Compatibility Layer**: Offers compatibility layers for applications that use other SSL/TLS libraries, easing the migration process.

6. **Community and Commercial Support**:
   - **Active Community**: WolfSSL has an active community and extensive documentation, providing valuable resources for developers.
   - **Commercial Support**: Offers professional support and consulting services for enterprises, ensuring timely assistance and custom solutions.

### Comparison with Other Protocols and Libraries

1. **OpenSSL**:
   - **Size and Performance**: OpenSSL is more feature-rich but has a larger footprint and may not be as optimized for performance in resource-constrained environments as WolfSSL.
   - **Complexity**: OpenSSL's API can be more complex, making it harder to integrate and maintain.

2. **mbedTLS**:
   - **Similar Footprint**: Like WolfSSL, mbedTLS is designed for embedded systems, but WolfSSL's performance optimizations and additional features like post-quantum cryptography support can be advantageous.
   - **Commercial Support**: While mbedTLS also offers commercial support, WolfSSL's extensive professional services and regular updates may provide an edge.

3. **BoringSSL**:
   - **Google's Fork**: BoringSSL is a fork of OpenSSL maintained by Google, designed to be used internally by Google products. It may lack the same level of community support and documentation as WolfSSL.
   - **Feature Set**: BoringSSL strips out many features of OpenSSL, focusing on Google's specific needs, which might not align with all use cases.

### Specific Advantages in Lightway Core

1. **Optimized for Performance**:
   - Lightway Core aims to be a lightweight and efficient VPN protocol. WolfSSL's optimizations for performance align well with these goals, ensuring fast and secure connections.

2. **Security Features**:
   - The inclusion of modern and post-quantum cryptographic algorithms in WolfSSL enhances the security of Lightway Core, providing robust protection against current and future threats.

3. **Flexibility and Configurability**:
   - WolfSSL's highly configurable nature allows Lightway Core to be tailored for various deployment scenarios, from high-performance servers to low-power IoT devices.

4. **Ease of Integration**:
   - The simple API and extensive documentation of WolfSSL make it easier to integrate and maintain within the Lightway Core codebase, reducing development and maintenance overhead.

### Conclusion

WolfSSL is chosen for Lightway Core due to its lightweight design, high performance, strong security features, wide platform support, and ease of integration. These benefits make it a suitable choice for implementing a modern, efficient, and secure VPN protocol like Lightway.

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
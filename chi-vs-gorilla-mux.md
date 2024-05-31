### Why Choose Chi Router over Gorilla Mux?

While both Chi and Gorilla Mux are excellent routing libraries for Go, they have different design philosophies and feature sets that might make one more suitable than the other depending on your application's needs. Here are some reasons why you might choose Chi over Gorilla Mux:

### Chi Router

1. **Lightweight and Fast**:
   - **Performance**: Chi is designed to be lightweight and fast. It has minimal overhead, which makes it ideal for high-performance applications.
   - **Efficiency**: The internal architecture of Chi is optimized for speed, making it suitable for microservices and APIs where performance is critical.

2. **Modular Middleware**:
   - **Middleware Chain**: Chi provides a flexible and composable middleware stack. Middleware can be applied at the router level, at route groups, or on individual routes.
   - **Built-in Middleware**: Chi includes a wide range of built-in middleware for common tasks like logging, recovering from panics, request ID generation, timeouts, and more.

3. **Context Management**:
   - **Request Contexts**: Chi uses the standard `context.Context` for request-scoped values, deadlines, and cancelation signals, aligning with Go's idiomatic context management.
   - **Path Parameters**: Chi seamlessly integrates path parameters with the request context, making it easy to access them.

4. **Nested Routers**:
   - **Sub-Routers**: Chi allows you to create nested routers, which helps in organizing routes and applying middleware to specific groups of routes.

5. **API Stability and Maintenance**:
   - **Actively Maintained**: Chi has a stable API and is actively maintained. It keeps up with the latest Go language features and best practices.
   - **Community and Documentation**: Chi has good documentation and a supportive community, making it easier to get help and find examples.

### Gorilla Mux

1. **Feature-Rich**:
   - **Versatility**: Gorilla Mux is a feature-rich router with support for advanced routing patterns, including variables in paths, regex matching, and more.
   - **Mature Library**: It is a mature library with a wide range of features that can handle complex routing requirements.

2. **Middleware Support**:
   - **Flexible Middleware**: While Gorilla Mux supports middleware, it is often used in conjunction with other Gorilla packages like `handlers` and `context`.

3. **Compatibility**:
   - **Established Ecosystem**: Gorilla Mux is part of the larger Gorilla web toolkit, which includes other packages like `sessions`, `securecookie`, and `websocket`. This makes it easier to build web applications with a consistent set of tools.

### Choosing Between Chi and Gorilla Mux

- **Use Chi if**:
  - You need a lightweight, fast, and efficient router.
  - You want to use idiomatic Go features like `context.Context` extensively.
  - You prefer a flexible and composable middleware system.
  - You are building high-performance microservices or APIs.

- **Use Gorilla Mux if**:
  - You need a feature-rich router with advanced routing capabilities.
  - You are building a web application that requires complex routing patterns.
  - You want to leverage the Gorilla web toolkit for additional functionality.

### Example Using Chi Router

Here’s a more detailed example using the Chi router to set up a secure API server with TLS:

#### main.go
```go
package main

import (
    "crypto/tls"
    "log"
    "net/http"
    "time"

    "github.com/go-chi/chi/v5"
    "github.com/go-chi/chi/v5/middleware"
)

func main() {
    // Create a new Chi router
    r := chi.NewRouter()

    // Middleware stack
    r.Use(middleware.RequestID)
    r.Use(middleware.RealIP)
    r.Use(middleware.Logger)
    r.Use(middleware.Recoverer)
    r.Use(middleware.Timeout(60 * time.Second))

    // Define a simple handler
    r.Get("/hello", func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("Hello, TLS secured world!"))
    })

    // Create a TLS configuration
    tlsConfig := &tls.Config{
        MinVersion: tls.VersionTLS12,
        // Add more configurations as needed, e.g., CipherSuites, Certificates, etc.
    }

    // Create a new HTTP server with TLS configuration
    srv := &http.Server{
        Addr:      ":8443",
        Handler:   r,
        TLSConfig: tlsConfig,
    }

    // Start the server with TLS
    log.Println("Starting server on https://localhost:8443")
    if err := srv.ListenAndServeTLS("server.crt", "server.key"); err != nil {
        log.Fatalf("Failed to start server: %v", err)
    }
}
```

### Conclusion

Chi and Gorilla Mux both have their strengths. Chi's design focuses on simplicity, performance, and idiomatic use of Go's context package, making it a great choice for high-performance APIs and microservices. Gorilla Mux offers a rich set of features suitable for complex web applications and benefits from being part of the broader Gorilla web toolkit.

Choose the router that best fits your project's needs and architectural goals.
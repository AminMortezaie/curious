## Provide and Invoke

In Uber's Fx, `Provide` and `Invoke` are used to manage dependencies and control the lifecycle of your application. They serve different purposes:

### `Provide`

- **Purpose**: `Provide` is used to register constructors for dependencies. These constructors are functions that return one or more values (usually pointers to structs or interfaces) that Fx will manage.
- **Usage**: When you call `Provide`, you're telling Fx how to construct the necessary components of your application. Fx will call these constructors to create the instances it needs to satisfy dependencies.
- **Example**: If you need to construct a database connection, a repository, and a service, you would use `Provide` to register the constructors for each of these.

```go
func NewDB(config *config.Config) (*gorm.DB, error) {
    dsn := config.DatabaseURL
    db, err := gorm.Open(postgres.Open(dsn), &gorm.Config{})
    if err != nil {
        return nil, err
    }
    return db, nil
}

func NewRepository(db *gorm.DB) *MyRepository {
    return &MyRepository{DB: db}
}

func NewService(repo *MyRepository) *MyService {
    return &MyService{Repo: repo}
}

// Register the constructors with Provide
app := fx.New(
    fx.Provide(
        config.NewConfig,
        NewDB,
        NewRepository,
        NewService,
    ),
)
```

### `Invoke`

- **Purpose**: `Invoke` is used to execute functions that need to run when your application starts. These functions typically depend on the components that were provided via `Provide`.
- **Usage**: When you call `Invoke`, you're instructing Fx to call a function at startup. This function can take dependencies as parameters, and Fx will automatically provide the necessary instances.
- **Example**: If you need to start an HTTP server or run some initialization logic that depends on your previously constructed components, you would use `Invoke`.

```go
func StartServer(lc fx.Lifecycle, service *MyService) {
    lc.Append(fx.Hook{
        OnStart: func(context.Context) error {
            // Logic to start the server
            go func() {
                log.Println("Server started")
            }()
            return nil
        },
        OnStop: func(ctx context.Context) error {
            // Logic to stop the server
            log.Println("Server stopped")
            return nil
        },
    })
}

// Register the start logic with Invoke
app := fx.New(
    fx.Provide(
        config.NewConfig,
        NewDB,
        NewRepository,
        NewService,
    ),
    fx.Invoke(StartServer),
)
```

### Differences

1. **Registration vs. Execution**:
   - **Provide**: Registers how to create dependencies.
   - **Invoke**: Registers functions to be executed at startup, which may depend on the provided dependencies.

2. **Lifecycle Management**:
   - **Provide**: Focuses on constructing components and managing their lifecycle.
   - **Invoke**: Focuses on executing startup logic and managing lifecycle hooks (e.g., what to do on start and stop).

3. **Purpose and Timing**:
   - **Provide**: Defines how dependencies are constructed before the application runs.
   - **Invoke**: Defines actions to be performed after all dependencies have been constructed and the application is starting.

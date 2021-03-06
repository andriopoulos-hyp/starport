# Directory Structure

A typical Cosmos SDK directory structure is as follows

```
x/{module}
├── client
│   ├── cli
│   │   ├── query.go
│   │   └── tx.go
│   └── rest
│       ├── query.go
│       └── tx.go
├── exported
│   └── exported.go
├── keeper
│   ├── invariants.go
│   ├── genesis.go
│   ├── keeper.go
│   ├── ...
│   └── querier.go
│   └── grpc_query.go
├── types
│   ├── codec.go
│   ├── errors.go
│   ├── events.go
│   ├── expected_keepers.go
│   ├── genesis.go
│   ├── keys.go
│   ├── msgs.go
│   ├── params.go
│   ├── types.proto
│   ├── ...
│   └── querier.go
│   └── {module_name}.pb.go
│   └── query.pb.go
│   └── genesis.pb.go
├── simulation
│   ├── decoder.go
│   ├── genesis.go
│   ├── operations.go
│   ├── params.go
│   └── proposals.go
├── abci.go
├── handler.go
├── ...
└── module.go
```

`abci.go`: The module's BeginBlocker and EndBlocker implementations (if any).

`client/`: The module's CLI and REST client functionality implementation and testing.

`exported/`: The module's exported types -- typically type interfaces. If a module relies on other module keepers, it is expected to receive them as interface contracts through the expected_keepers.go (which are detailed below) design to avoid having a direct dependency on the implementing module. However, these contracts can define methods that operate on and/or return types that are specific to the contract's implementing module and this is where exported/ comes into play. Types defined here allow for expected_keepers.go in other modules to define contracts that use single canonical types. This pattern allows for code to remain DRY and also alleviates import cycle chaos.

`handler.go`: The module's message handlers.

`keeper/`: The module's keeper implementation along with any auxiliary implementations such as the querier and invariants.

`types/`: The module's type definitions such as messages, KVStore keys, parameter types, Protocol Buffer definitions, and expected_keepers.go contracts.

`module.go`: The module's implementation of the AppModule and AppModuleBasic interfaces.

# ABD – Implementation Levels

Recommended implementation profile for ABD projects. The `abd/` directory resides in the project root, alongside the native framework structure. This profile is a default, not a constraint: deviations are explicit human decisions.

## Directory Structure

```
abd/
├── actions/
│   ├── user/
│   │   ├── CreateUser.php
│   │   ├── UpdateUser.php
│   │   └── DeleteUser.php
│   └── order/
│       ├── CreateOrder.php
│       └── CancelOrder.php
├── service/
│   ├── UserManagement.php
│   └── OrderManagement.php
└── integration/
    ├── auth/
    │   └── TwoFactorAuth.php
    ├── document/
    │   └── XmlGenerator.php
    └── payment/
        └── StripeGateway.php
```

## Implementation Levels

**Action Level** 
Contains the Actions derived from the Actions List. Each Action is a dedicated file, grouped by Domain. Implements the defined contract: input, output, business rules. By default, calls only Service Level methods.

**Service Level** 
Contains the business logic methods called by Actions. Methods are abstract and framework-independent. By default, no direct Models, no database queries, no external library calls. Delegates persistence to the framework Data Access Layer and external dependencies to the Integration Level.

Example for Action `user:create`:

```
UserManagement::GetUserByEmail
UserManagement::InsertUser
UserManagement::LoginUser
```

**Integration Level** 
Contains wrappers for external dependencies: third-party libraries, external APIs, authentication services, document generators. Isolates external dependencies from the Service Level. If a library is replaced, the change is localized here.

## Rules

- By default, Actions call only Service Level methods. Deviations are explicit human decisions.
- By default, Service Level methods do not contain direct external dependencies. Deviations are explicit human decisions.
- By default, Integration Level isolates all third-party libraries and external APIs. Deviations are explicit human decisions.
- The framework lives in its native structure. `abd/` sits alongside it.
- If the coding agent detects a level violation, it warns the developer before proceeding, without blocking generation.
- This structure is a recommended implementation profile for ABD, not a core constraint of the method.

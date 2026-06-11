# Presets

Presets are pre-compiled Steering Files for specific combinations of technology stack and architectural choices. Each Preset represents a validated configuration ready to be adopted as a starting point in an ABD project.

Presets exist because compiling Steering Files from scratch for a stack already used in a previous project is repetitive work with no added value. More importantly, Steering Files for areas like security and UI carry real risk when written without a concrete reference: critical sections get omitted, conventions become inconsistent, and developers new to ABD have no example of what a correctly compiled file looks like.

A Preset is not a template. It is a configuration that has been used successfully in at least one real ABD project. It is not produced theoretically.

---

## Structure

Presets are organized by Steering File:

```
presets/
├── security-model/
│   ├── security-model-laravel-sanctum.md
│   ├── security-model-laravel-jwt.md
│   └── ...
├── ui-spec/
│   ├── ui-spec-tailwind-shadcn.md
│   └── ...
├── stack-config/
│   ├── stack-config-laravel-postgresql.md
│   └── ...
└── database-schema/
    └── database-schema-starter-kit.md
```

The `database-schema` Preset is an exception: it is not tied to a specific stack. It provides the foundational entities that recur in almost every project — users, roles, permissions — and is named `database-schema-starter-kit.md` accordingly.

---

## Naming Convention

Every Preset follows the convention:

```
[steering-file-name]-[primary-stack]-[key-library].md
```

Examples: `security-model-laravel-sanctum.md`, `ui-spec-tailwind-shadcn.md`, `stack-config-laravel-postgresql.md`.

The exception is `database-schema-starter-kit.md`, which covers common entities across stacks rather than a specific combination.

---

## Priority

Not all Steering Files benefit equally from standardization. The priority for Preset production reflects where the risk of starting from scratch is highest:

| Steering File | Priority | Reason |
|---|---|---|
| `security-model.md` | High | Incomplete security configurations are a critical risk when there is no concrete reference |
| `ui-spec.md` | High | Without a shared reference, applications in the same ecosystem develop inconsistent visual conventions |
| `stack-config.md` | Medium | Common stack combinations produce structurally identical files |
| `database-schema.md` | Medium | Recurring entities like users, roles, and permissions appear in almost every project |
| `integration-config.md` | Low | External dependencies vary too much between projects for generic Presets to be useful |

---

## How to Use a Preset

1. Identify the Preset that matches your project stack.
2. Copy it into the `abd/` directory of your project as a starting point.
3. Customise it with the specifics of your project: roles, permissions, components, naming conventions.
4. Submit the customised file to human validation during the Execution Layer Setup, as you would with any other Steering File.

A Preset accelerates compilation and reduces the risk of omissions. It does not replace validation. Every Preset includes a header that states the stack it covers, the reference library versions, and the date of last validation. Check the date before adopting a Preset: if the reference libraries have released breaking changes since the last validation, review the relevant sections before use.

---

## Preset Header Format

Every Preset begins with a standard header:

```
---
preset: [steering-file-name]-[stack]-[library]
stack: [primary stack and version]
libraries: [key libraries and versions]
last-validated: [YYYY-MM-DD]
status: current | outdated
---
```

A Preset marked `status: outdated` has not been updated after breaking changes in its reference libraries. It can still be used as a structural reference but requires careful review before adoption.

---

## Contributing a Preset

A Preset is contributed when a Steering File configuration has been used successfully in a real ABD project. Theoretical configurations are not accepted.

To contribute a Preset:
1. Produce the Steering File in your project following standard ABD Execution Layer Setup.
2. Validate and ratify it as part of the project.
3. Strip project-specific details, replacing them with clearly marked placeholders.
4. Add the standard header with stack, library versions, and validation date.
5. Submit it to the `presets/` directory under the appropriate subfolder.

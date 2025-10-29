# HelixUI Specification

## What is HelixUI?

HelixUI is a universal, platform-agnostic intermediate representation for user interfaces. Think of it as "assembly language for UIs" - a standardized JSON format that any UI can be expressed in and compiled to any platform.

## Files in this Directory

- **SPECIFICATION.md** - Complete HelixUI v0.1 specification
- **schema.json** (coming next) - JSON Schema for validation

## Quick Start

### Example: Simple Button

```json
{
  "version": "0.1",
  "id": "hello-world",
  "root": {
    "type": "button",
    "label": "Click me",
    "action": "actions.sayHello"
  },
  "actions": {
    "sayHello": {
      "type": "http",
      "method": "POST",
      "url": "/api/hello"
    }
  }
}
```

### Example: User Form

See `/examples/user-form.json` for a complete form with validation.

### Example: Data Table

See `/examples/user-list.json` for a full CRUD interface with table, search, and pagination.

### Example: Dashboard

See `/examples/dashboard.json` for a complex multi-widget layout.

## Core Concepts

### 1. Component Types (15 primitives)

**Layout:**
- `container`, `flex`, `grid`, `stack`

**Widgets:**
- `text`, `input`, `button`, `image`, `link`, `select`, `checkbox`, `radio`

**Semantic:**
- `table`, `modal`, `form`

### 2. Data Binding

Use JSON Path syntax:

```json
{
  "type": "text",
  "binding": "$.user.name"
}
```

### 3. Actions

Define interactions:

```json
{
  "actions": {
    "saveUser": {
      "type": "http",
      "method": "POST",
      "url": "/api/users",
      "body": "$.formData"
    }
  }
}
```

### 4. Layout System

Flexbox-based layout:

```json
{
  "layout": {
    "strategy": "flex",
    "direction": "column",
    "gap": 16,
    "padding": 24
  }
}
```

## Design Principles

1. **Minimal Core** - 15 primitive types, extensible
2. **Platform Agnostic** - Compiles to HTMX, React, SwiftUI, etc.
3. **Human Readable** - JSON format, clear structure
4. **Type Safe** - JSON Schema validation
5. **Declarative** - Describe what, not how

## Use Cases

- **Website cloning** - Extract UI from any site
- **Design to code** - Convert Figma/screenshots to code
- **Cross-platform** - Write once, compile everywhere
- **AI generation** - Structured output for LLMs
- **Component libraries** - Framework-agnostic widgets

## Next Steps

1. Create JSON Schema for validation
2. Build reference compiler (HelixUI → HTMX)
3. Test with real-world examples
4. Iterate based on findings

## Version

Current: **v0.1** (2025-10-27)

## License

MIT

# HelixJSON Specification

A declarative, platform-agnostic specification for building user interfaces.

## What is HelixJSON?

HelixJSON is a JSON-based specification for describing user interfaces that can be compiled to any target platform or framework.

Think of it as:
- **OpenAPI** for APIs → **HelixJSON** for UIs
- Write once in JSON, compile to any platform (web, mobile, desktop, embedded)
- Platform-agnostic, framework-agnostic, language-agnostic
- Designed for both humans and AI

## Specification

- **[Core Specification](spec/SPECIFICATION.md)** - v0.2
  - 15 component types (Container, Flex, Grid, Input, Button, etc.)
  - Reactive data binding
  - HTTP actions
  - Conditional rendering
  - Advanced input features (clearable, debounce, counter, etc.)

### File Format

- **Extension:** `.helix`
- **Format:** JSON (UTF-8)
- **MIME Type:** `application/json` or `application/vnd.helix+json`

## Examples

See the [examples/](examples/) directory for reference implementations:

- `login.helix` - Login form with validation
- `dashboard.helix` - Complex dashboard layout
- `user-form.helix` - Multi-step form
- `product-list.helix` - Data display with actions
- `modal-workflow.helix` - Modal dialogs
- And 4 more examples...

## Quick Example

```json
{
  "version": "0.2",
  "id": "hello-world",
  "data": {
    "initial": {
      "name": ""
    }
  },
  "root": {
    "type": "flex",
    "direction": "column",
    "gap": 16,
    "children": [
      {
        "type": "input",
        "variant": "text",
        "binding": "$.name",
        "placeholder": "Enter your name"
      },
      {
        "type": "text",
        "content": "Hello, {{$.name}}!",
        "variant": "h1"
      }
    ]
  }
}
```

This can be compiled to any target platform with reactive data binding.

## Why HelixJSON?

1. **Platform Independence** - Not tied to web, mobile, or desktop - compile to any platform
2. **Framework Independence** - Not tied to React, Vue, SwiftUI, or any specific framework
3. **AI-Friendly** - JSON format perfect for LLM generation
4. **Type-Safe** - Clear schema, easy to validate
5. **Future-Proof** - Change target platform or framework without rewriting
6. **Truly Portable** - Write once, deploy anywhere

## Use Cases

- **Multi-Platform Applications** - Single UI definition → web, iOS, Android, desktop, embedded
- **AI Code Generation** - LLMs generate HelixJSON JSON for any platform
- **No-Code/Low-Code Tools** - Visual builders output universal HelixJSON
- **Design Systems** - Standard UI language across all platforms and teams
- **Legacy Migration** - Convert old UIs to modern platforms without manual rewriting
- **Cross-Platform Development** - Maintain one spec, compile to native experiences

## Optimization Guidelines

HelixJSON files can be optimized for transmission and storage using standard techniques:

### Development
- Use formatted JSON with whitespace for readability
- Store in source control as `.helix`
- Human-readable, easy to edit and review

### Production
**Minification:**
```bash
# Remove whitespace (30-40% reduction)
jq -c . input.helix > output.helix
```
*Note: Compilers automatically handle both formatted and minified JSON - no special detection needed.*

**Compression:**
```bash
# Gzip compression (80-90% reduction from original)
gzip input.helix          # Creates input.helix.gz

# Brotli compression (slightly better than gzip)
brotli input.helix        # Creates input.helix.br
```

**HTTP Transport:**
- Use `Content-Encoding: gzip` or `Content-Encoding: br` headers
- Most web servers and CDNs handle this automatically
- Reduces bandwidth and improves load times

### Binary Formats (Optional)
Compilers may optionally support binary serialization formats:
- **MessagePack** - Fast binary JSON alternative (~60-70% size reduction)
- **CBOR** - Concise Binary Object Representation
- **Protocol Buffers** - If schema is pre-shared

Binary formats trade human-readability for size and parse speed. Recommended for:
- High-performance applications
- Bandwidth-constrained environments
- Embedded systems
- Mobile applications

## Contributing

This is a specification. Contributions welcome for:
- Spec improvements and clarifications
- Additional examples
- Documentation
- Platform compiler implementations

## License

MIT

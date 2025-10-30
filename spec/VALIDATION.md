# HelixJSON Validation

## JSON Schema

The HelixJSON JSON Schema is available at `/spec/schema.json`.

## Validating Examples

### Using Node.js

```bash
npm install -g ajv-cli
ajv validate -s spec/schema.json -d "examples/*.json"
```

### Using Python

```bash
pip install jsonschema
python scripts/validate.py
```

### Using Online Tools

Visit [JSONSchemaValidator.net](https://www.jsonschemavalidator.net/) and paste:
- Schema: `/spec/schema.json`
- Data: Any example from `/examples/`

## Validation Status

**Schema Version:** 0.1
**Last Updated:** 2025-10-27

### Examples

- ✅ user-list.json - Valid
- ✅ user-form.json - Valid
- ✅ dashboard.json - Valid
- 🔜 settings.json - Pending
- 🔜 blog-editor.json - Pending
- 🔜 product-list.json - Pending
- 🔜 login.json - Pending
- 🔜 modal-workflow.json - Pending

## Common Validation Errors

### Missing Required Fields

```json
{
  "error": "Missing required property: version",
  "fix": "Add 'version': '0.1' to root object"
}
```

### Invalid Component Type

```json
{
  "error": "Invalid component type: 'textarea'",
  "fix": "Use 'input' with variant or 'richtext' (if available)"
}
```

### Invalid Action Type

```json
{
  "error": "Invalid action type: 'submit'",
  "fix": "Use 'http' action with POST method"
}
```

## Schema Coverage

The schema validates:

- ✅ Document structure (version, id, root)
- ✅ All 15 component types
- ✅ Layout system (flex, grid, stack)
- ✅ Data bindings
- ✅ All 5 action types
- ✅ Validation rules
- ✅ Platform hints
- ✅ Styling (inline, classes, refs)

## Future Enhancements

- [ ] Validate JSON Path syntax
- [ ] Validate template strings ({{variable}})
- [ ] Validate action references
- [ ] Validate binding paths exist in data schema
- [ ] Lint for best practices

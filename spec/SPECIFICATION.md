# HelixJSON Specification v0.1

## Overview

HelixJSON is a JSON-based, platform-agnostic intermediate representation for user interfaces. It enables:

- **Universal UI description** - Define UI once, compile to any platform
- **Declarative structure** - Component hierarchy, layout, styling, and behavior
- **Type-safe** - Validated with JSON Schema
- **Extensible** - Plugin system for custom components

**Design Principles:**
1. Minimal core, maximum extensibility
2. Human-readable, machine-parsable JSON
3. Platform-agnostic (compiles to HTMX, React, SwiftUI, etc.)
4. Three abstraction levels: low-level layout, mid-level widgets, high-level semantics

---

## JSON Document Structure

```json
{
  "$schema": "https://helixui.org/v0.1/schema.json",
  "version": "0.1",
  "id": "unique-ui-identifier",

  "meta": {
    "name": "Human Readable Name",
    "description": "Description of this UI",
    "author": "Author or tool name",
    "created": "2025-10-27T00:00:00Z"
  },

  "root": {
    "type": "container",
    "children": []
  },

  "data": {
    "schema": {},
    "initial": {}
  },

  "actions": {},

  "styles": {}
}
```

### Top-Level Fields

- **`$schema`** (string, optional) - JSON Schema URL for validation
- **`version`** (string, required) - HelixJSON spec version (e.g., "0.1")
- **`id`** (string, required) - Unique identifier for this UI
- **`meta`** (object, optional) - Metadata about the UI
- **`root`** (object, required) - Root component of the UI tree
- **`data`** (object, optional) - Data schema and initial values
- **`actions`** (object, optional) - Named actions/event handlers
- **`styles`** (object, optional) - Reusable style definitions

---

## Core Primitive Types

HelixJSON v0.1 includes 15 core primitive types organized into three categories:

### 1. Layout Primitives (Structure)

These define how child components are arranged.

#### `container`

Generic container for grouping elements.

```json
{
  "type": "container",
  "layout": {
    "strategy": "flex",
    "direction": "column",
    "gap": 16,
    "padding": 16
  },
  "children": []
}
```

**Properties:**
- `layout` (object) - Layout configuration (see Layout System)
- `children` (array) - Child components
- `className` (string) - CSS class or Tailwind classes
- `style` (object) - Inline styles

#### `flex`

Flexbox layout container.

```json
{
  "type": "flex",
  "direction": "row",
  "justify": "space-between",
  "align": "center",
  "gap": 8,
  "children": []
}
```

**Properties:**
- `direction` (string) - "row" | "column" | "row-reverse" | "column-reverse"
- `justify` (string) - "start" | "end" | "center" | "space-between" | "space-around"
- `align` (string) - "start" | "end" | "center" | "stretch" | "baseline"
- `gap` (number) - Space between children in pixels
- `wrap` (boolean) - Allow wrapping

#### `grid`

CSS Grid layout container.

```json
{
  "type": "grid",
  "columns": "1fr 2fr 1fr",
  "rows": "auto",
  "gap": 16,
  "children": []
}
```

**Properties:**
- `columns` (string) - Grid template columns (CSS syntax)
- `rows` (string) - Grid template rows
- `gap` (number) - Gap between cells
- `columnGap` (number) - Horizontal gap
- `rowGap` (number) - Vertical gap

#### `stack`

Simplified vertical or horizontal stack (syntactic sugar for flex).

```json
{
  "type": "stack",
  "direction": "vertical",
  "spacing": 16,
  "children": []
}
```

**Properties:**
- `direction` (string) - "vertical" | "horizontal"
- `spacing` (number) - Space between children
- `alignment` (string) - "start" | "center" | "end" | "stretch"

### 2. Widget Primitives (UI Elements)

Core interactive and display components.

#### `text`

Text display with variants for typography.

```json
{
  "type": "text",
  "content": "Hello World",
  "variant": "h1",
  "binding": "$.title"
}
```

**Properties:**
- `content` (string) - Text content (if not bound)
- `binding` (string) - Data binding path (JSON Path)
- `variant` (string) - "h1" | "h2" | "h3" | "h4" | "h5" | "h6" | "body" | "caption" | "label"
- `className` (string) - CSS classes

#### `input`

Text input field with variants.

```json
{
  "type": "input",
  "variant": "text",
  "binding": "$.username",
  "placeholder": "Enter username",
  "required": true,
  "onChange": "actions.validateUsername"
}
```

**Properties:**
- `variant` (string) - "text" | "email" | "password" | "number" | "url" | "tel" | "search"
- `binding` (string) - Two-way data binding path
- `placeholder` (string) - Placeholder text
- `required` (boolean) - Required field
- `disabled` (boolean) - Disabled state
- `onChange` (string) - Action to trigger on change
- `validation` (object) - Validation rules

#### `button`

Clickable button.

```json
{
  "type": "button",
  "label": "Submit",
  "variant": "primary",
  "action": "actions.submit",
  "disabled": false
}
```

**Properties:**
- `label` (string) - Button text
- `variant` (string) - "primary" | "secondary" | "danger" | "ghost" | "link"
- `action` (string) - Action to trigger on click
- `disabled` (boolean) - Disabled state
- `type` (string) - "button" | "submit" | "reset"

#### `image`

Image display.

```json
{
  "type": "image",
  "src": "https://example.com/image.jpg",
  "alt": "Description",
  "width": 200,
  "height": 200
}
```

**Properties:**
- `src` (string) - Image URL or binding
- `alt` (string) - Alt text
- `width` (number) - Width in pixels
- `height` (number) - Height in pixels
- `fit` (string) - "cover" | "contain" | "fill" | "none"

#### `link`

Hyperlink or navigation link.

```json
{
  "type": "link",
  "label": "Go to page",
  "href": "/page",
  "target": "_self"
}
```

**Properties:**
- `label` (string) - Link text
- `href` (string) - URL or route
- `target` (string) - "_self" | "_blank" | "_parent" | "_top"
- `action` (string) - Action instead of navigation

#### `select`

Dropdown/select input.

```json
{
  "type": "select",
  "binding": "$.role",
  "options": [
    {"value": "admin", "label": "Administrator"},
    {"value": "user", "label": "User"}
  ],
  "placeholder": "Select role"
}
```

**Properties:**
- `binding` (string) - Data binding path
- `options` (array) - Array of {value, label} objects
- `optionsBinding` (string) - Bind options from data
- `placeholder` (string) - Placeholder text
- `multiple` (boolean) - Allow multiple selection

#### `checkbox`

Checkbox input.

```json
{
  "type": "checkbox",
  "binding": "$.agreed",
  "label": "I agree to terms",
  "required": true
}
```

**Properties:**
- `binding` (string) - Data binding path (boolean)
- `label` (string) - Checkbox label
- `required` (boolean) - Required field

#### `radio`

Radio button group.

```json
{
  "type": "radio",
  "binding": "$.plan",
  "options": [
    {"value": "free", "label": "Free"},
    {"value": "pro", "label": "Pro"}
  ]
}
```

**Properties:**
- `binding` (string) - Data binding path
- `options` (array) - Array of {value, label} objects

### 3. Semantic Widgets (High-Level)

Complex, opinionated components.

#### `table`

Data table with sorting, filtering, and actions.

```json
{
  "type": "table",
  "binding": "$.users",
  "columns": [
    {
      "key": "name",
      "label": "Name",
      "sortable": true
    },
    {
      "key": "email",
      "label": "Email"
    },
    {
      "key": "actions",
      "type": "actions",
      "actions": [
        {"label": "Edit", "action": "actions.editUser"},
        {"label": "Delete", "action": "actions.deleteUser", "variant": "danger"}
      ]
    }
  ],
  "pagination": {
    "pageSize": 25,
    "showSizeSelector": true
  }
}
```

**Properties:**
- `binding` (string) - Array data binding
- `columns` (array) - Column definitions
- `sortable` (boolean) - Enable sorting
- `filterable` (boolean) - Enable filtering
- `pagination` (object) - Pagination config
- `selectable` (boolean) - Enable row selection

#### `modal`

Modal/dialog overlay.

```json
{
  "type": "modal",
  "id": "confirm-delete",
  "title": "Confirm Delete",
  "size": "medium",
  "children": [
    {
      "type": "text",
      "content": "Are you sure you want to delete this item?"
    }
  ],
  "actions": [
    {"label": "Cancel", "action": "actions.closeModal"},
    {"label": "Delete", "action": "actions.confirmDelete", "variant": "danger"}
  ]
}
```

**Properties:**
- `id` (string) - Modal identifier
- `title` (string) - Modal title
- `size` (string) - "small" | "medium" | "large" | "fullscreen"
- `children` (array) - Modal content
- `actions` (array) - Footer action buttons
- `closeOnBackdrop` (boolean) - Close when clicking outside

#### `form`

Form container with validation.

```json
{
  "type": "form",
  "binding": "$.formData",
  "onSubmit": "actions.submitForm",
  "children": [],
  "validation": {
    "email": {
      "type": "email",
      "required": true,
      "message": "Valid email required"
    }
  }
}
```

**Properties:**
- `binding` (string) - Form data binding
- `onSubmit` (string) - Submit action
- `validation` (object) - Validation rules
- `children` (array) - Form fields

---

## Component Features (v0.2)

Component features are optional enhancements that modify component behavior without changing their fundamental type. Features are platform-agnostic and each compiler determines the best implementation approach.

### Universal Features

These features can be applied to any component:

#### `tooltip`

Displays help text on hover or focus.

```json
{
  "type": "button",
  "label": "Delete",
  "tooltip": {
    "content": "Permanently delete this item",
    "position": "top",
    "delay": 500
  }
}
```

**Properties:**
- `content` (string) - Tooltip text
- `position` (string) - "top" | "bottom" | "left" | "right" (default: "top")
- `delay` (number) - Show delay in milliseconds (default: 0)

#### `skeleton`

Loading placeholder that appears while content loads.

```json
{
  "type": "text",
  "binding": "$.userName",
  "skeleton": {
    "enabled": "$.isLoading",
    "width": "150px",
    "animation": "pulse"
  }
}
```

**Properties:**
- `enabled` (boolean|string) - When to show skeleton (can be binding)
- `width` (string) - Placeholder width
- `height` (string) - Placeholder height
- `animation` (string) - "pulse" | "wave" | "none" (default: "pulse")

### Input Features

Enhanced functionality for `input` components:

#### `clearable`

Adds a button to clear the input value.

```json
{
  "type": "input",
  "binding": "$.searchQuery",
  "clearable": true
}
```

**Type:** `boolean` (default: false)

#### `prefix` / `suffix`

Adds text or icons before/after the input.

```json
{
  "type": "input",
  "binding": "$.amount",
  "prefix": "$",
  "suffix": "USD"
}
```

**Type:** `string` or `object`

With icon:
```json
{
  "type": "input",
  "prefix": {
    "icon": "search",
    "className": "text-gray-500"
  }
}
```

#### `mask` / `format`

Formats input text according to a pattern.

```json
{
  "type": "input",
  "variant": "tel",
  "mask": "(###) ###-####"
}
```

**Predefined formats:**
- `"phone"` - Phone number: (123) 456-7890
- `"currency"` - Currency: $1,234.56
- `"date"` - Date: MM/DD/YYYY
- `"ssn"` - SSN: ###-##-####
- `"creditCard"` - Card: #### #### #### ####

**Custom mask:** Use `#` for digits, `A` for letters, `*` for any character

#### `debounce`

Delays triggering onChange action until user stops typing.

```json
{
  "type": "input",
  "binding": "$.searchQuery",
  "onChange": "actions.search",
  "debounce": 300
}
```

**Type:** `number` (milliseconds)

#### `toggleVisibility`

For password inputs, adds an eye icon to show/hide password.

```json
{
  "type": "input",
  "variant": "password",
  "binding": "$.password",
  "toggleVisibility": true
}
```

**Type:** `boolean` (default: false)

#### `counter`

Shows character count, typically for textarea.

```json
{
  "type": "input",
  "variant": "textarea",
  "binding": "$.bio",
  "maxLength": 280,
  "counter": true
}
```

**Type:** `boolean` (default: false)

**Note:** Requires `maxLength` to be set.

### Button Features

Enhanced functionality for `button` components:

#### `icon`

Adds an icon to the button.

```json
{
  "type": "button",
  "label": "Save",
  "icon": {
    "name": "save",
    "position": "left"
  }
}
```

**Type:** `string` or `object`

Simple form:
```json
{
  "icon": "save"
}
```

Full form:
```json
{
  "icon": {
    "name": "save",
    "position": "left",  // "left" | "right" (default: "left")
    "size": 16
  }
}
```

#### `loading`

Shows loading state with optional spinner.

```json
{
  "type": "button",
  "label": "Submit",
  "loading": "$.isSubmitting",
  "loadingText": "Submitting...",
  "loadingIcon": true
}
```

**Properties:**
- `loading` (boolean|string) - Loading state (can be binding)
- `loadingText` (string) - Text to show when loading (optional)
- `loadingIcon` (boolean) - Show spinner icon (default: true)

### Feature Composition

Multiple features can be combined:

```json
{
  "type": "input",
  "variant": "search",
  "binding": "$.query",
  "placeholder": "Search users...",
  "clearable": true,
  "debounce": 300,
  "prefix": {
    "icon": "search"
  },
  "tooltip": {
    "content": "Type at least 2 characters to search"
  }
}
```

### Platform Implementation Notes

Features are intentionally high-level. Each compiler determines the best implementation:

- **Web (Alpine.js/HTMX):** Use Alpine directives and CSS
- **React:** Use hooks and component libraries
- **iOS (SwiftUI):** Use native modifiers
- **Android:** Use Material Design components

---

## Layout System

The layout system defines how components are positioned and sized.

### Layout Object

```json
{
  "layout": {
    "strategy": "flex",
    "direction": "column",
    "justify": "start",
    "align": "stretch",
    "gap": 16,
    "padding": 24,
    "margin": 0
  }
}
```

### Properties

- **`strategy`** (string) - "flex" | "grid" | "stack" | "absolute"
- **`direction`** (string) - "row" | "column" | "row-reverse" | "column-reverse"
- **`justify`** (string) - Main axis alignment
- **`align`** (string) - Cross axis alignment
- **`gap`** (number | object) - Space between children
- **`padding`** (number | object) - Internal padding
- **`margin`** (number | object) - External margin

### Spacing Notation

Spacing can be a number (applies to all sides) or an object:

```json
{
  "padding": 16
}
```

```json
{
  "padding": {
    "top": 24,
    "right": 16,
    "bottom": 24,
    "left": 16
  }
}
```

Shorthand:
```json
{
  "padding": {"vertical": 24, "horizontal": 16}
}
```

---

## Data Binding System

HelixJSON uses JSON Path syntax for data bindings.

### Binding Syntax

- **`$.propertyName`** - Root-level property
- **`$.user.profile.name`** - Nested property
- **`$.items[*]`** - Array of all items
- **`$.items[0].title`** - Specific array item
- **`$.users[?(@.active)]`** - Filtered items (advanced)

### One-Way Binding (Display)

```json
{
  "type": "text",
  "binding": "$.user.name"
}
```

### Two-Way Binding (Input)

```json
{
  "type": "input",
  "binding": "$.user.email",
  "onChange": "actions.validateEmail"
}
```

### Array Binding

```json
{
  "type": "table",
  "binding": "$.users"
}
```

### Data Schema

Define data structure and initial values:

```json
{
  "data": {
    "schema": {
      "$schema": "http://json-schema.org/draft-07/schema#",
      "type": "object",
      "properties": {
        "user": {
          "type": "object",
          "properties": {
            "name": {"type": "string"},
            "email": {"type": "string", "format": "email"}
          }
        }
      }
    },
    "initial": {
      "user": {
        "name": "",
        "email": ""
      }
    }
  }
}
```

---

## Action System

Actions define interactions and side effects.

### Action Types

#### HTTP Action

Make HTTP requests.

```json
{
  "actions": {
    "fetchUsers": {
      "type": "http",
      "method": "GET",
      "url": "/api/users",
      "onSuccess": {
        "binding": "$.users",
        "action": "actions.showSuccessToast"
      },
      "onError": {
        "action": "actions.showErrorToast"
      }
    }
  }
}
```

**Properties:**
- `type` - "http"
- `method` - "GET" | "POST" | "PUT" | "DELETE" | "PATCH"
- `url` - Endpoint URL (can use template: `/api/users/{{id}}`)
- `body` - Request body (can reference data: `"$.formData"`)
- `headers` - Custom headers
- `onSuccess` - Success handler
- `onError` - Error handler

#### Navigation Action

Navigate to a different route/page.

```json
{
  "actions": {
    "goToUser": {
      "type": "navigate",
      "url": "/users/{{userId}}",
      "target": "_self"
    }
  }
}
```

**Properties:**
- `type` - "navigate"
- `url` - Target URL
- `target` - "_self" | "_blank"

#### Modal Action

Open/close modals.

```json
{
  "actions": {
    "openDeleteModal": {
      "type": "modal",
      "operation": "open",
      "modalId": "confirm-delete",
      "data": {
        "itemId": "$.selectedItem.id"
      }
    }
  }
}
```

**Properties:**
- `type` - "modal"
- `operation` - "open" | "close" | "toggle"
- `modalId` - Modal identifier
- `data` - Data to pass to modal

#### Data Action

Manipulate data directly.

```json
{
  "actions": {
    "resetForm": {
      "type": "data",
      "operation": "set",
      "binding": "$.formData",
      "value": {}
    }
  }
}
```

**Properties:**
- `type` - "data"
- `operation` - "set" | "merge" | "push" | "remove"
- `binding` - Target data path
- `value` - Value to set/merge

#### Composite Action

Chain multiple actions.

```json
{
  "actions": {
    "deleteUser": {
      "type": "composite",
      "actions": [
        "actions.confirmDelete",
        "actions.callDeleteAPI",
        "actions.refreshList",
        "actions.closeModal"
      ]
    }
  }
}
```

**Properties:**
- `type` - "composite"
- `actions` - Array of action references
- `stopOnError` - Stop chain if action fails

---

## Styling System

HelixJSON supports three styling approaches:

### 1. Inline Styles

```json
{
  "type": "container",
  "style": {
    "backgroundColor": "#f5f5f5",
    "borderRadius": 8,
    "boxShadow": "0 2px 4px rgba(0,0,0,0.1)"
  }
}
```

### 2. CSS Classes (Tailwind Hints)

```json
{
  "type": "container",
  "className": "bg-gray-100 rounded-lg shadow-md p-4"
}
```

### 3. Named Styles (Reusable)

```json
{
  "styles": {
    "card": {
      "backgroundColor": "#ffffff",
      "borderRadius": 8,
      "padding": 16,
      "boxShadow": "0 2px 8px rgba(0,0,0,0.1)"
    }
  },
  "root": {
    "type": "container",
    "styleRef": "card"
  }
}
```

---

## Validation

Components can specify validation rules:

```json
{
  "type": "input",
  "binding": "$.email",
  "validation": {
    "type": "email",
    "required": true,
    "message": "Valid email is required"
  }
}
```

### Validation Rules

- **`required`** (boolean) - Field is required
- **`type`** (string) - "email" | "url" | "number" | "integer"
- **`minLength`** (number) - Minimum string length
- **`maxLength`** (number) - Maximum string length
- **`min`** (number) - Minimum numeric value
- **`max`** (number) - Maximum numeric value
- **`pattern`** (string) - Regex pattern
- **`message`** (string) - Error message

---

## Platform Hints

Provide platform-specific guidance without breaking abstraction:

```json
{
  "platformHints": {
    "web": {
      "responsive": true,
      "breakpoints": {
        "mobile": 640,
        "tablet": 768,
        "desktop": 1024
      }
    },
    "mobile": {
      "scrollable": true,
      "keyboardAvoidance": true
    },
    "desktop": {
      "windowTitle": "User Management",
      "minWidth": 800,
      "minHeight": 600
    }
  }
}
```

---

## Complete Example: User Management Page

```json
{
  "$schema": "https://helixui.org/v0.1/schema.json",
  "version": "0.1",
  "id": "user-management",

  "meta": {
    "name": "User Management",
    "description": "Admin interface for managing users",
    "author": "HelixJSON",
    "created": "2025-10-27T00:00:00Z"
  },

  "root": {
    "type": "container",
    "layout": {
      "strategy": "flex",
      "direction": "column",
      "gap": 24,
      "padding": 24
    },
    "children": [
      {
        "type": "flex",
        "justify": "space-between",
        "align": "center",
        "children": [
          {
            "type": "text",
            "variant": "h1",
            "content": "Users"
          },
          {
            "type": "button",
            "label": "Add User",
            "variant": "primary",
            "action": "actions.openAddUserModal"
          }
        ]
      },
      {
        "type": "table",
        "binding": "$.users",
        "columns": [
          {
            "key": "name",
            "label": "Name",
            "sortable": true
          },
          {
            "key": "email",
            "label": "Email",
            "sortable": true
          },
          {
            "key": "role",
            "label": "Role"
          },
          {
            "key": "actions",
            "type": "actions",
            "actions": [
              {
                "label": "Edit",
                "action": "actions.editUser"
              },
              {
                "label": "Delete",
                "action": "actions.deleteUser",
                "variant": "danger"
              }
            ]
          }
        ],
        "pagination": {
          "pageSize": 25,
          "showSizeSelector": true
        }
      }
    ]
  },

  "data": {
    "schema": {
      "type": "object",
      "properties": {
        "users": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "id": {"type": "string"},
              "name": {"type": "string"},
              "email": {"type": "string"},
              "role": {"type": "string"}
            }
          }
        }
      }
    },
    "initial": {
      "users": []
    }
  },

  "actions": {
    "fetchUsers": {
      "type": "http",
      "method": "GET",
      "url": "/api/users",
      "onSuccess": {
        "binding": "$.users"
      }
    },
    "openAddUserModal": {
      "type": "modal",
      "operation": "open",
      "modalId": "add-user-modal"
    },
    "editUser": {
      "type": "navigate",
      "url": "/users/{{id}}/edit"
    },
    "deleteUser": {
      "type": "composite",
      "actions": [
        {
          "type": "modal",
          "operation": "open",
          "modalId": "confirm-delete"
        }
      ]
    }
  }
}
```

---

## Version History

- **v0.1** (2025-10-27) - Initial specification
  - 15 core primitive types
  - Layout system (flex, grid, stack)
  - Data binding with JSON Path
  - Action system (http, navigate, modal, data, composite)
  - Basic validation
  - Platform hints

---

## Future Considerations (Post-v0.1)

Items explicitly deferred:

- Animation system
- Transitions
- Theming system
- Advanced validation (async, custom validators)
- Conditional rendering
- Loops/iterations (beyond array binding)
- Complex state management
- Hooks/lifecycle events
- Custom component definitions
- Plugin system

These will be added based on real-world usage feedback.

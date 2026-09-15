# FSCSS Internals

Understand how FSCSS works under the hood. Learn the parsing, compilation, and output generation.

## Architecture Overview

```
FSCSS Input (.fscss)
        ↓
    Tokenizer
        ↓
    Parser
        ↓
    AST (Abstract Syntax Tree)
        ↓
    Transformer
        ↓
    Generator
        ↓
CSS Output (.css)
```

## The Compilation Pipeline

### 1. Tokenization

The tokenizer breaks input into tokens:

```fscss
$primary: #2563eb;

.button {
  background: $primary;
}
```

Becomes tokens:

```
[$, primary, :, #2563eb, ;]
[.button, {, background, :, $primary, ;, }]
```

### 2. Parsing

The parser builds an AST from tokens:

```json
{
  "type": "stylesheet",
  "rules": [
    {
      "type": "variable",
      "name": "primary",
      "value": "#2563eb"
    },
    {
      "type": "rule",
      "selector": ".button",
      "declarations": [
        {
          "property": "background",
          "value": "$primary"
        }
      ]
    }
  ]
}
```

### 3. Transformation

The transformer resolves variables and processes features:

```json
{
  "type": "rule",
  "selector": ".button",
  "declarations": [
    {
      "property": "background",
      "value": "#2563eb"
    }
  ]
}
```

### 4. Code Generation

The generator outputs CSS:

```css
.button {
  background: #2563eb;
}
```

## Key Components

### Variable Resolution

```fscss
// Before
$primary: #2563eb;
.btn { background: $primary; }

// After
.btn { background: #2563eb; }
```

### @fun Expansion

```fscss
// Before
@fun(card) {
  background: white;
  padding: 1rem;
}
.card { @fun(card); }

// After
.card { background: white; padding: 1rem; }
```

### @define Processing

```fscss
// Before
@define button(bg, color) {
  background: @use(bg);
  color: @use(color);
}
.btn { @button(#2563eb, white); }

// After
.btn { background: #2563eb; color: white; }
```

### Vendor Prefixing

```fscss
// Before
.box { -*transform: rotate(45deg); }

// After
.box {
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  transform: rotate(45deg);
}
```

## Source Code Structure

```
fscss/
├── src/
│   ├── tokenizer.js      # Lexical analysis
│   ├── parser.js         # Syntax analysis
│   ├── ast.js            # AST definitions
│   ├── transformer.js    # AST transformations
│   ├── generator.js      # Code generation
│   └── compiler.js       # Main compiler
├── lib/
│   └── utils.js          # Utility functions
├── tests/
│   └── *.test.js         # Test files
└── package.json
```

## Contributing to Internals

### 1. Understand the Codebase

Read through the source files to understand the flow.

### 2. Add Tests

```javascript
// tests/variable.test.js
const { test, expect } = require('@jest/globals');

test('variable resolution', () => {
  const input = '$primary: #2563eb; .btn { background: $primary; }';
  const output = compile(input);
  expect(output).toContain('.btn { background: #2563eb; }');
});
```

### 3. Submit Pull Requests

Follow the contribution guidelines in [Contributing to FSCSS](./contributing-to-fscss.md).

## Exercises

1. Trace through the compilation of a simple FSCSS file
2. Read the tokenizer source code
3. Add a new test for variable resolution

---

**Next: [Building a Compiler](./building-a-fscss-compiler.md)**

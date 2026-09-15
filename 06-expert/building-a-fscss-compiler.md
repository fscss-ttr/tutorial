# Building a FSCSS Compiler

Create your own FSCSS compiler from scratch. Learn how CSS preprocessors work.

## Project Structure

```
my-fscss-compiler/
├── src/
│   ├── tokenizer.js
│   ├── parser.js
│   ├── transformer.js
│   ├── generator.js
│   └── compiler.js
├── tests/
│   └── compiler.test.js
├── cli.js
└── package.json
```

## Step 1: Tokenizer

Break input into tokens:

```javascript
// src/tokenizer.js
function tokenize(input) {
  const tokens = [];
  let i = 0;

  while (i < input.length) {
    // Variable
    if (input[i] === '$') {
      let name = '';
      i++; // Skip $
      while (i < input.length && input[i] !== ':' && input[i] !== ' ') {
        name += input[i];
        i++;
      }
      tokens.push({ type: 'variable', name });
      continue;
    }

    // Selector
    if (input[i] === '.') {
      let selector = '';
      while (i < input.length && input[i] !== '{') {
        selector += input[i];
        i++;
      }
      tokens.push({ type: 'selector', value: selector.trim() });
      continue;
    }

    // Property
    if (input[i] === ':') {
      let property = '';
      i++; // Skip :
      while (i < input.length && input[i] !== ';' && input[i] !== '}') {
        property += input[i];
        i++;
      }
      tokens.push({ type: 'property', value: property.trim() });
      continue;
    }

    i++;
  }

  return tokens;
}

module.exports = { tokenize };
```

## Step 2: Parser

Build AST from tokens:

```javascript
// src/parser.js
function parse(tokens) {
  const ast = {
    type: 'stylesheet',
    rules: []
  };

  let i = 0;
  while (i < tokens.length) {
    // Variable declaration
    if (tokens[i].type === 'variable') {
      const variable = {
        type: 'variable',
        name: tokens[i].name,
        value: tokens[i + 2].value // Skip : token
      };
      ast.rules.push(variable);
      i += 3;
      continue;
    }

    // Rule
    if (tokens[i].type === 'selector') {
      const rule = {
        type: 'rule',
        selector: tokens[i].value,
        declarations: []
      };
      i++; // Skip selector

      // Parse declarations
      while (i < tokens.length && tokens[i].type !== 'selector') {
        if (tokens[i].type === 'property') {
          rule.declarations.push({
            property: tokens[i].value,
            value: tokens[i + 1].value
          });
          i += 2;
        } else {
          i++;
        }
      }

      ast.rules.push(rule);
      continue;
    }

    i++;
  }

  return ast;
}

module.exports = { parse };
```

## Step 3: Transformer

Resolve variables and process features:

```javascript
// src/transformer.js
function transform(ast) {
  const variables = {};

  // Collect variables
  ast.rules.forEach(rule => {
    if (rule.type === 'variable') {
      variables[rule.name] = rule.value;
    }
  });

  // Transform rules
  ast.rules = ast.rules
    .filter(rule => rule.type !== 'variable')
    .map(rule => {
      if (rule.type === 'rule') {
        rule.declarations = rule.declarations.map(decl => {
          // Resolve variables
          if (decl.value.startsWith('$')) {
            const varName = decl.value.slice(1);
            decl.value = variables[varName] || decl.value;
          }
          return decl;
        });
      }
      return rule;
    });

  return ast;
}

module.exports = { transform };
```

## Step 4: Generator

Output CSS:

```javascript
// src/generator.js
function generate(ast) {
  let css = '';

  ast.rules.forEach(rule => {
    if (rule.type === 'rule') {
      css += `${rule.selector} {\n`;
      rule.declarations.forEach(decl => {
        css += `  ${decl.property}: ${decl.value};\n`;
      });
      css += '}\n\n';
    }
  });

  return css.trim();
}

module.exports = { generate };
```

## Step 5: Main Compiler

```javascript
// src/compiler.js
const { tokenize } = require('./tokenizer');
const { parse } = require('./parser');
const { transform } = require('./transformer');
const { generate } = require('./generator');

function compile(input) {
  const tokens = tokenize(input);
  const ast = parse(tokens);
  const transformed = transform(ast);
  const css = generate(transformed);
  return css;
}

module.exports = { compile };
```

## Step 6: CLI

```javascript
// cli.js
const fs = require('fs');
const { compile } = require('./src/compiler');

const args = process.argv.slice(2);

if (args.length < 2) {
  console.error('Usage: my-fscss <input> <output>');
  process.exit(1);
}

const input = fs.readFileSync(args[0], 'utf8');
const css = compile(input);
fs.writeFileSync(args[1], css);

console.log(`Compiled ${args[0]} → ${args[1]}`);
```

## Step 7: Tests

```javascript
// tests/compiler.test.js
const { compile } = require('../src/compiler');

test('compiles variables', () => {
  const input = '$primary: #2563eb; .btn { background: $primary; }';
  const output = compile(input);
  expect(output).toContain('.btn {');
  expect(output).toContain('background: #2563eb;');
});

test('compiles rules', () => {
  const input = '.card { padding: 1rem; border-radius: 8px; }';
  const output = compile(input);
  expect(output).toContain('.card {');
  expect(output).toContain('padding: 1rem;');
});
```

## Enhancements

### Add @fun Support

```javascript
// In transformer.js
function processFunRules(ast) {
  const funs = {};

  // Collect @fun definitions
  ast.rules.forEach(rule => {
    if (rule.type === 'fun') {
      funs[rule.name] = rule.declarations;
    }
  });

  // Apply @fun
  ast.rules.forEach(rule => {
    if (rule.type === 'fun-apply') {
      const fun = funs[rule.name];
      if (fun) {
        rule.type = 'rule';
        rule.declarations = [...fun];
      }
    }
  });

  return ast;
}
```

### Add Vendor Prefixing

```javascript
// In transformer.js
function addVendorPrefixes(ast) {
  const prefixes = ['-webkit-', '-moz-', '-ms-', '-o-'];

  ast.rules.forEach(rule => {
    rule.declarations.forEach(decl => {
      if (decl.property.startsWith('-*')) {
        const baseProperty = decl.property.slice(2);
        const prefixDeclarations = prefixes.map(prefix => ({
          property: prefix + baseProperty,
          value: decl.value
        }));
        prefixDeclarations.push({ property: baseProperty, value: decl.value });
        // Replace with prefixed versions
      }
    });
  });

  return ast;
}
```

## Exercises

1. Add support for nested selectors
2. Implement @define blocks
3. Add loops and conditionals

---

**Next: [Community Ecosystem](./community-ecosystem.md)**

# Future of FSCSS

Explore what's coming next for FSCSS and the roadmap ahead.

## Current State (v1.2.x)

### Stable Features

- Variables
- Style stores (@fun)
- Parameterized blocks (@define)
- Vendor prefixing
- Imports
- Inline style
- Runtime and build-time compilation

### Growing Ecosystem

- Community plugins
- Design system packages
- IDE support
- Documentation

## Planned Features

### Short-Term (v1.3.x)

#### 1. Enhanced @define

```fscss
@define card(variant: default, shadow: md) {
  background: @use(variant) == 'dark' ? #1e293b : white;
  box-shadow: @use(shadow) == 'lg' ? $shadow-lg : $shadow-md;
}
```

#### 2. Better Error Messages

More helpful error reporting:

```
Error: Variable $primary not found
  --> src/styles.fscss:15:3
   |
15 |   background: $primary;
   |              ^^^^^^^^ Did you forget to define $primary?
```

#### 3. Source Maps

Generate source maps for debugging:

```bash
fscss src/styles.fscss dist/styles.css --source-map
```

### Medium-Term (v2.0.x)

#### 1. Native Nesting

```fscss
.card {
  background: white;

  &-title {
    font-size: 1.25rem;
  }

  &-body {
    padding: 1.5rem;
  }
}
```

#### 2. Functions

```fscss
@function double($value) {
  @return $value * 2;
}

.box {
  width: double(50px);  /* 100px */
}
```

#### 3. Mixins with Content Blocks

```fscss
@mixin responsive($breakpoint) {
  @media (min-width: $breakpoint) {
    @content;
  }
}

.container {
  padding: 1rem;

  @include responsive(768px) {
    padding: 2rem;
  }
}
```

### Long-Term (v3.0.x)

#### 1. TypeScript Integration

```typescript
// fscss.config.ts
import { defineConfig } from 'fscss';

export default defineConfig({
  variables: {
    primary: '#2563eb',
  },
  plugins: [],
});
```

#### 2. AST API

```javascript
import { parse, transform, generate } from 'fscss';

const ast = parse(input);
const transformed = transform(ast);
const css = generate(transformed);
```

#### 3. Custom Syntax

```fscss
// Define custom syntax
@syntax my-syntax {
  @token variable { $[a-z]+ }
  @token property { [a-z-]+ }
}

@use my-syntax;
```

## Community Roadmap

### Voting on Features

The community votes on priorities:

1. Native nesting (78% support)
2. Functions (65% support)
3. Better error messages (92% support)
4. Source maps (71% support)

### Contributing to Roadmap

Join discussions:

- [GitHub Discussions](https://github.com/fscss-ttr/FSCSS/discussions)
- [Roadmap Issue](https://github.com/fscss-ttr/FSCSS/issues/roadmap)

## FSCSS vs Other Preprocessors

### Unique Advantages

1. **Simplicity** — Easier to learn than Sass
2. **Shorthand** — `-*` vendor prefixing
3. **Runtime option** — Use without build tools
4. **Lightweight** — Minimal footprint

### Future Differentiation

- Better integration with modern frameworks
- Enhanced developer experience
- Improved performance

## How to Influence the Future

### 1. Report Issues

Found a bug? Have an idea? Open an issue.

### 2. Submit Pull Requests

Want a feature? Build it and submit a PR.

### 3. Community Feedback

Share your experience, suggest improvements.

### 4. Spread the Word

Tell others about FSCSS, write tutorials, give talks.

## Exercises

1. Read through the GitHub issues for feature requests
2. Vote on the community roadmap
3. Contribute to a feature discussion

---

**Congratulations! You've completed the Expert section. Ready to build? Check out the [Projects](../07-projects/README.md).**

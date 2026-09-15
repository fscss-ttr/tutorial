# Vendor Prefixing

Automatically add browser prefixes to CSS properties. Write once, work everywhere.

## Wildcard Prefixing

The `-*` prefix adds all vendor prefixes automatically:

```fscss
.box {
  -*transform: rotate(45deg);
}
```

Compiles to:

```css
.box {
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  transform: rotate(45deg);
}
```

## Common Prefixed Properties

### Transform

```fscss
.element {
  -*transform: translateX(50px);
  -*transform: scale(1.2);
  -*transform: rotate(180deg);
}
```

### Transition

```fscss
.button {
  -*transition: all 0.3s ease;
  -*transition: background-color 0.2s ease, transform 0.2s ease;
}
```

### Flexbox

```fscss
.container {
  -*display: flex;
  -*flex-wrap: wrap;
  -*align-items: center;
  -*justify-content: space-between;
}
```

### Grid

```fscss
.grid {
  -*display: grid;
  -*grid-template-columns: repeat(3, 1fr);
  -*grid-gap: 1rem;
}
```

### Appearance

```fscss
.input {
  -*appearance: none;
}
```

### User Select

```fscss
.text {
  -*user-select: none;
}
```

### Filter

```fscss
.image {
  -*filter: blur(5px);
  -*filter: brightness(1.2);
  -*filter: contrast(1.5);
}
```

## Properties That Need Prefixing

| Property | Prefixes Needed |
|----------|-----------------|
| `transform` | Yes |
| `transition` | Yes |
| `animation` | Yes |
| `display: flex` | Yes |
| `display: grid` | Yes |
| `appearance` | Yes |
| `user-select` | Yes |
| `filter` | Yes |
| `backdrop-filter` | Yes |
| `clip-path` | Yes |

## Properties That Don't Need Prefixing

| Property | Prefixes Needed |
|----------|-----------------|
| `color` | No |
| `font-size` | No |
| `margin` | No |
| `padding` | No |
| `border` | No |
| `background` | No |
| `width` | No |
| `height` | No |

## Batch Prefixing

Prefix multiple properties at once:

```fscss
.hero {
  -*display: flex;
  -*flex-direction: column;
  -*align-items: center;
  -*justify-content: center;
  -*transform: translateY(-50%);
  -*transition: all 0.3s ease;
}
```

## Prefix with Variables

```fscss
$transition-speed: 0.3s;

.button {
  -*transition: all $transition-speed ease;
}
```

## Prefix in @fun

```fscss
@fun(flex-center) {
  display: flex;
  align-items: center;
  justify-content: center;
}

@fun(card) {
  background: white;
  border-radius: 8px;
  -*transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.centered { @fun.flex-center; }
.card { @fun.card; }
```

## Prefix in @define

```fscss
@define animated-element(animation) {
  -*animation: @use(animation);
  -*transition: all 0.3s ease;
}

.fade-in {
  @animated-element(fadeIn 0.5s ease forwards);
}
```

## Best Practices

### 1. Use for Modern Properties Only

```fscss
/* Good: Modern property */
 -*transform: rotate(45deg);

/* Avoid: Already standard */
-color: red;  /* No prefix needed */
```

### 2. Don't Over-Prefix

```fscss
/* Good: Prefix what needs it */
.button {
  -*transition: all 0.3s ease;
}

/* Avoid: Prefix everything */
.button {
  -color: white;  /* Unnecessary */
  -padding: 12px 24px;  /* Unnecessary */
}
```

### 3. Check Browser Support

Use `-*` for properties that still need prefixes in your target browsers.

## Exercises

1. Add vendor prefixes to a flexbox layout
2. Prefix a CSS animation
3. Create a prefixed grid system

---

**Next: [Inline Style](./inline-style.md)**

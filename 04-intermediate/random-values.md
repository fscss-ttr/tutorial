# Random Values

Generate random values in FSCSS for creative and dynamic designs.

## Random Function

Generate random numbers:

```fscss
.random-width {
  width: random(100) * 1%;  /* Random width between 1% and 100% */
}
```

## Random Colors

Generate random background colors:

```fscss
@for $i from 1 to 10 {
  .color-#{$i} {
    background: rgb(random(256), random(256), random(256));
  }
}
```

## Random Positions

Create randomly positioned elements:

```fscss
@for $i from 1 to 5 {
  .dot-#{$i} {
    position: absolute;
    left: random(100) * 1%;
    top: random(100) * 1%;
    width: 10px;
    height: 10px;
    background: #2563eb;
    border-radius: 50%;
  }
}
```

## Random Sizes

Generate random element sizes:

```fscss
@for $i from 1 to 8 {
  .box-#{$i} {
    width: random(200) + 50px;
    height: random(200) + 50px;
    background: hsl(random(360), 70%, 60%);
  }
}
```

## Random Opacity

Apply random transparency:

```fscss
@for $i from 1 to 10 {
  .opacity-#{$i} {
    opacity: random(100) / 100;
  }
}
```

## Random Rotation

Rotate elements randomly:

```fscss
@for $i from 1 to 12 {
  .rotate-#{$i} {
    -*transform: rotate(random(360) * 1deg);
  }
}
```

## Practical Examples

### Decorative Dots Background

```fscss
.dots-background {
  position: relative;
  overflow: hidden;
}

@for $i from 1 to 20 {
  .dot-#{$i} {
    position: absolute;
    width: random(20) + 5px;
    height: random(20) + 5px;
    background: rgba(37, 99, 235, random(50) / 100);
    border-radius: 50%;
    left: random(100) * 1%;
    top: random(100) * 1%;
  }
}
```

### Random Grid Heights

```fscss
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
}

@for $i from 1 to 12 {
  .grid-item-#{$i} {
    height: random(200) + 100px;
    background: hsl(random(360), 60%, 70%);
    border-radius: 8px;
  }
}
```

### Random Animations

```fscss
@for $i from 1 to 6 {
  @keyframes float-#{$i} {
    0%, 100% {
      -*transform: translateY(0) rotate(0deg);
    }
    50% {
      -*transform: translateY(random(50) - 25px) rotate(random(20) - 10deg);
    }
  }

  .float-#{$i} {
    animation: float-#{$i} random(3) + 2s ease-in-out infinite;
    animation-delay: random(2) * 1s;
  }
}
```

## Tips

### 1. Seed for Consistency

Use a seed for reproducible random values:

```fscss
random(100, seed: 123);  /* Always produces the same sequence */
```

### 2. Limit Ranges

Keep random values within reasonable bounds:

```fscss
/* Good */
width: random(100) + 100px;  /* 100px to 200px */

/* Avoid */
width: random(10000)px;  /* Too unpredictable */
```

### 3. Use for Decoration Only

Random values work best for:

- Decorative backgrounds
- Particle effects
- Creative art projects
- Visual experiments

Avoid for:

- Core layout
- Typography
- Critical UI elements

## Exercises

1. Create a confetti effect with random colors and positions
2. Build a random grid layout with varying heights
3. Design a particle animation system

---

**Next: [Vendor Prefixing](./vendor-prefixing.md)**

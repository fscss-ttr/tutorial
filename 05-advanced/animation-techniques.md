# Animation Techniques

Create smooth, performant animations with FSCSS. Enhance user experience with motion.

## Transitions

### Basic Transition

```fscss
.button {
  background: #2563eb;
  -*transition: all 0.3s ease;

  &:hover {
    background: #1d4ed8;
  }
}
```

### Specific Properties

```fscss
.card {
  -*transition: transform 0.3s ease, box-shadow 0.3s ease;

  &:hover {
    -*transform: translateY(-4px);
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);
  }
}
```

### Transition Timing

```fscss
/* Ease (default) */
.button { -*transition: all 0.3s ease; }

/* Linear */
.button { -*transition: all 0.3s linear; }

/* Ease-in */
.button { -*transition: all 0.3s ease-in; }

/* Ease-out */
.button { -*transition: all 0.3s ease-out; }

/* Ease-in-out */
.button { -*transition: all 0.3s ease-in-out; }
```

## Keyframe Animations

### Fade In

```fscss
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.fade-in {
  -*animation: fadeIn 0.5s ease forwards;
}
```

### Slide Up

```fscss
@keyframes slideUp {
  from {
    -*transform: translateY(20px);
    opacity: 0;
  }
  to {
    -*transform: translateY(0);
    opacity: 1;
  }
}

.slide-up {
  -*animation: slideUp 0.5s ease forwards;
}
```

### Scale

```fscss
@keyframes scale {
  from {
    -*transform: scale(0.95);
    opacity: 0;
  }
  to {
    -*transform: scale(1);
    opacity: 1;
  }
}

.scale-in {
  -*animation: scale 0.3s ease forwards;
}
```

### Bounce

```fscss
@keyframes bounce {
  0%, 20%, 53%, 80%, 100% {
    -*transform: translateY(0);
  }
  40%, 43% {
    -*transform: translateY(-20px);
  }
  70% {
    -*transform: translateY(-10px);
  }
  90% {
    -*transform: translateY(-4px);
  }
}

.bounce {
  -*animation: bounce 1s ease infinite;
}
```

### Pulse

```fscss
@keyframes pulse {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}

.pulse {
  -*animation: pulse 2s ease-in-out infinite;
}
```

## Practical Examples

### Loading Spinner

```fscss
@keyframes spin {
  from {
    -*transform: rotate(0deg);
  }
  to {
    -*transform: rotate(360deg);
  }
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e28f0f;
  border-top-color: #2563eb;
  border-radius: 50%;
  -*animation: spin 1s linear infinite;
}
```

### Hover Effects

```fscss
.card {
  -*transition: transform 0.3s ease, box-shadow 0.3s ease;

  &:hover {
    -*transform: translateY(-8px);
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
  }
}

.button {
  -*transition: transform 0.2s ease, box-shadow 0.2s ease;

  &:hover {
    -*transform: scale(1.05);
    box-shadow: 0 4px 12px rgba(37, 99, 235, 0.4);
  }

  &:active {
    -*transform: scale(0.98);
  }
}
```

### Page Transitions

```fscss
.page-enter {
  -*animation: slideUp 0.5s ease forwards;
}

.page-leave {
  -*animation: slideDown 0.5s ease forwards;
}

@keyframes slideDown {
  from {
    -*transform: translateY(0);
    opacity: 1;
  }
  to {
    -*transform: translateY(20px);
    opacity: 0;
  }
}
```

## Performance Tips

### 1. Animate Transform and Opacity

```fscss
/* Good: GPU accelerated */
.box {
  -*transition: transform 0.3s ease, opacity 0.3s ease;
}

/* Avoid: Triggers layout */
.box {
  -*transition: width 0.3s ease, height 0.3s ease;
}
```

### 2. Use Will-Change Sparingly

```fscss
.box {
  will-change: transform;
}
```

### 3. Keep Animations Short

```fscss
/* Good: 200-500ms */
.button { -*transition: all 0.2s ease; }

/* Avoid: Too slow */
.button { -*transition: all 2s ease; }
```

## Exercises

1. Create a loading spinner animation
2. Build hover effects for cards
3. Design page transition animations

---

**Next: [Grid Layouts](./grid-layouts.md)**

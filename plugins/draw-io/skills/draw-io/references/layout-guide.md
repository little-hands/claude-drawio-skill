# Layout Guide

Layout principles, coordinate calculations, and placement patterns for draw.io diagrams.

## Layout Principles

### 1. Group and Place Elements by Zone

Group related elements into zones:

```text
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│ Aggregate A │  │ Aggregate B │  │ Services /  │
│   (left)    │  │  (center)   │  │ Repositories│
│             │  │             │  │  (right)    │
└─────────────┘  └─────────────┘  └─────────────┘
```

### 2. Distribute Edge Connection Points

When multiple edges connect to the same element, use `exitX`/`exitY`/`entryX`/`entryY` to distribute connection points:

```xml
<!-- Connect from top -->
exitX=0.5;exitY=0;entryX=0.5;entryY=1

<!-- Connect from upper-left side -->
exitX=0;exitY=0.25;entryX=1;entryY=0.25

<!-- Connect from lower-left side -->
exitX=0;exitY=0.75;entryX=1;entryY=0.75
```

**Coordinate meaning:**
- `exitX=0` left edge, `exitX=1` right edge, `exitX=0.5` center
- `exitY=0` top edge, `exitY=1` bottom edge, `exitY=0.5` center

### 3. Place Vertical Relationships Vertically

Arrange parent-child relationships and compositions vertically:

```text
  ┌──────┐
  │Parent│
  └──┬───┘
     │
  ┌──┴───┐
  │ Child│
  └──────┘
```

### 4. Place Horizontal References Horizontally

Arrange cross-aggregate references horizontally and connect with straight lines:

```text
┌──────────┐      ┌──────────┐
│Aggregate │ ───→ │Aggregate │
│    A     │      │    B     │
└──────────┘      └──────────┘
```

### 5. Place Services and Repositories at the Edge

Place domain services and repositories at the edge of the diagram (e.g., right side) to minimize crossing lines to aggregates.

## Layout Example (Domain Model)

```text
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  ┌──────────┐  ┌──────────┐  ┌───────────────────────┐  │
│  │Aggregate │─→│Aggregate │  │  Domain Services      │  │
│  │    A     │  │    B     │  │  · PricingPolicy      │  │
│  │ ┌──────┐ │  │ ┌──────┐ │  │  · StockAllocator    │  │
│  │ │ Root │ │  │ │ Root │ │  └────────────┬──────────┘  │
│  │ └──┬───┘ │  │ └──┬───┘ │               │             │
│  │    │     │  │    │     │  ┌────────────┴──────────┐  │
│  │ ┌──┴───┐ │  │ ┌──┴───┐ │  │  Repositories        │  │
│  │ │Child │ │  │ │Child │ │  │  · OrderRepository    │  │
│  │ └──────┘ │  │ └──────┘ │  │  · ProductRepository  │  │
│  └──────────┘  └──────────┘  └───────────────────────┘  │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │ Legend                                          │    │
│  └─────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
```

## Boundary Coordinate Calculations

When enclosing child elements with a separate boundary, be careful about **coordinate system differences**.

**Problem**: Child elements use parent-relative coordinates, but boundaries often need absolute coordinates.

**Formula:**
```
Absolute coordinate = Parent coordinate + Child relative coordinate
```

### Example: Creating a scope boundary in a use case diagram

```xml
<!-- Parent (system boundary) -->
<mxCell id="uc-boundary" ... parent="1">
  <mxGeometry x="280" y="100" ... />
</mxCell>

<!-- Children (use cases) - relative coordinates from parent -->
<mxCell id="uc1" ... parent="uc-boundary">
  <mxGeometry y="60" height="60" ... />  <!-- relative: y=60~120 -->
</mxCell>
<mxCell id="uc2" ... parent="uc-boundary">
  <mxGeometry y="150" height="60" ... /> <!-- relative: y=150~210 -->
</mxCell>
```

**Converting to absolute coordinates:**
- uc1: 100 (parent y) + 60 = 160, bottom = 160 + 60 = 220
- uc2: 100 (parent y) + 150 = 250, bottom = 250 + 60 = 310

**Boundary coordinates (with 10px margin):**
```xml
<!-- Scope boundary - specified in absolute coordinates -->
<mxCell id="scope-boundary" ... parent="1">
  <mxGeometry y="150" height="170" ... />  <!-- 160-10 ~ 310+10 -->
</mxCell>
```

### Checklist

1. Check the `parent` of the target elements → relative or absolute coordinates?
2. Add the parent's coordinates to convert to absolute coordinates
3. Add margin (~10px) to determine the boundary range

# SVG Attributes and Scaling Guide

## Table of Contents
- [Introduction to SVG](#introduction-to-svg)
- [Essential SVG Attributes](#essential-svg-attributes)
- [ViewBox and Coordinate Systems](#viewbox-and-coordinate-systems)
- [SVG Scaling Behavior](#svg-scaling-behavior)
- [Basic SVG Shapes](#basic-svg-shapes)
- [Accessibility in SVG](#accessibility-in-svg)
- [Best Practices](#best-practices)

---

## Introduction to SVG

SVG (Scalable Vector Graphics) is an XML-based vector image format for two-dimensional graphics. Unlike raster images (PNG, JPG), SVG images can scale to any size without loss of quality.

### Key Benefits of Inline SVG:
- **Scalability**: Looks sharp at any resolution
- **Small file size**: Especially for simple graphics
- **Styleable with CSS**: Can be styled and animated
- **Accessible**: Can include semantic information for screen readers
- **Searchable**: Text in SVG is searchable and indexable
- **Programmable**: Can be manipulated with JavaScript

---

## Essential SVG Attributes

### `width` and `height`
Defines the display dimensions of the SVG element in the document.

```xml
<svg width="200" height="100">
  <!-- SVG content -->
</svg>
```

- Can use absolute units (px, pt, cm) or relative units (%, em, rem)
- Without units, defaults to pixels
- Can be omitted for fluid/responsive layouts

### `viewBox`
Defines the coordinate system and aspect ratio of the SVG viewport.

```xml
<svg viewBox="0 0 100 100">
  <!-- Content uses 0-100 coordinate system -->
</svg>
```

**Syntax**: `viewBox="min-x min-y width height"`
- `min-x, min-y`: Coordinates of the top-left corner
- `width, height`: Width and height of the viewport in user space

### `preserveAspectRatio`
Controls how the SVG scales when the aspect ratio differs from the viewBox.

```xml
<svg preserveAspectRatio="xMidYMid meet">
  <!-- Content -->
</svg>
```

**Common values**:
- `xMidYMid meet` (default): Scale uniformly, center content, show all
- `xMidYMid slice`: Scale uniformly, center content, may crop
- `none`: Stretch to fill, distort if necessary

### `xmlns` (XML Namespace)
Required for standalone SVG files; optional for inline SVG in HTML5.

```xml
<svg xmlns="http://www.w3.org/2000/svg">
  <!-- SVG content -->
</svg>
```

---

## ViewBox and Coordinate Systems

### Understanding ViewBox

The `viewBox` establishes a custom coordinate system for your SVG content, independent of its display size.

**Example 1: Basic ViewBox**
```xml
<svg width="200" height="200" viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="40" fill="blue"/>
</svg>
```
- The circle is drawn at coordinates (50, 50) with radius 40
- These coordinates are in the viewBox space (0-100)
- The SVG displays at 200×200px
- The circle automatically scales to fit

**Example 2: Zooming with ViewBox**
```xml
<!-- Full view -->
<svg width="200" height="200" viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="40" fill="blue"/>
</svg>

<!-- Zoomed in (shows center quarter) -->
<svg width="200" height="200" viewBox="25 25 50 50">
  <circle cx="50" cy="50" r="40" fill="blue"/>
</svg>
```

### Coordinate System Origin

By default:
- Origin (0, 0) is at the top-left corner
- X-axis increases to the right
- Y-axis increases downward (different from Cartesian coordinates)

**Changing the origin**:
```xml
<svg viewBox="-50 -50 100 100">
  <!-- Now (0,0) is at the center -->
  <circle cx="0" cy="0" r="40" fill="blue"/>
</svg>
```

---

## SVG Scaling Behavior

### 1. Fixed Size SVG
```xml
<svg width="300" height="200">
  <!-- Fixed display size -->
</svg>
```
- SVG has a fixed display size
- No automatic scaling

### 2. Responsive SVG (Fluid Width)
```xml
<svg width="100%" height="auto" viewBox="0 0 300 200">
  <!-- Scales with container -->
</svg>
```
- Width scales with parent container
- Maintains aspect ratio from viewBox

### 3. Intrinsic Sizing with ViewBox Only
```xml
<svg viewBox="0 0 300 200">
  <!-- No explicit width/height -->
</svg>
```
- Default to 300×150px in most browsers
- Can be sized with CSS

### 4. Scaling with CSS
```css
svg {
  width: 100%;
  height: auto;
  max-width: 500px;
}
```

### Scaling Strategies

**Strategy 1: Fixed Coordinate System, Flexible Display**
```xml
<svg width="100%" viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="40"/>
</svg>
```
- Draw using simple 0-100 coordinates
- Display scales responsively
- Maintains aspect ratio

**Strategy 2: Match ViewBox to Actual Dimensions**
```xml
<svg width="800" height="600" viewBox="0 0 800 600">
  <!-- 1:1 mapping between coordinates and pixels -->
</svg>
```
- Useful when designing at specific pixel dimensions
- Easy to reason about coordinates

**Strategy 3: Centered Coordinate System**
```xml
<svg width="200" height="200" viewBox="-100 -100 200 200">
  <!-- (0,0) is at center -->
  <circle cx="0" cy="0" r="50"/>
</svg>
```
- Simplifies centered designs
- Good for radial/symmetric graphics

---

## Basic SVG Shapes

### Circle
```xml
<circle cx="50" cy="50" r="40" fill="red" stroke="black" stroke-width="2"/>
```
- `cx, cy`: Center coordinates
- `r`: Radius

### Rectangle
```xml
<rect x="10" y="10" width="80" height="60" fill="blue" rx="5" ry="5"/>
```
- `x, y`: Top-left corner
- `width, height`: Dimensions
- `rx, ry`: Corner radius (optional)

### Ellipse
```xml
<ellipse cx="50" cy="50" rx="40" ry="25" fill="green"/>
```
- `cx, cy`: Center coordinates
- `rx, ry`: Horizontal and vertical radii

### Line
```xml
<line x1="0" y1="0" x2="100" y2="100" stroke="black" stroke-width="2"/>
```
- `x1, y1`: Start point
- `x2, y2`: End point
- Requires `stroke` (no `fill`)

### Polyline
```xml
<polyline points="0,0 50,25 100,0 150,25" fill="none" stroke="blue" stroke-width="2"/>
```
- `points`: Space or comma-separated coordinates
- Not closed (unlike polygon)

### Polygon
```xml
<polygon points="50,5 100,50 75,100 25,100 0,50" fill="yellow" stroke="black"/>
```
- `points`: Space or comma-separated coordinates
- Automatically closes the shape

### Path
Most powerful and flexible shape element.

```xml
<path d="M 10,10 L 90,90 M 90,10 L 10,90" stroke="red" stroke-width="2" fill="none"/>
```

**Path Commands**:
- `M x,y`: Move to
- `L x,y`: Line to
- `H x`: Horizontal line to
- `V y`: Vertical line to
- `C x1,y1 x2,y2 x,y`: Cubic Bézier curve
- `Q x1,y1 x,y`: Quadratic Bézier curve
- `A rx,ry rotation large-arc sweep x,y`: Arc
- `Z`: Close path

---

## Accessibility in SVG

### Why Accessibility Matters
SVG graphics should be accessible to users with screen readers and other assistive technologies.

### Accessibility Techniques

#### 1. Use `role="img"`
```xml
<svg role="img" aria-labelledby="title">
  <title id="title">Descriptive title</title>
  <!-- SVG content -->
</svg>
```

#### 2. Add `<title>` Element
Provides a short, descriptive name (like alt text).

```xml
<svg>
  <title>Company Logo</title>
  <circle cx="50" cy="50" r="40" fill="blue"/>
</svg>
```

#### 3. Add `<desc>` Element
Provides a longer, detailed description.

```xml
<svg role="img" aria-labelledby="chartTitle chartDesc">
  <title id="chartTitle">Sales Chart</title>
  <desc id="chartDesc">
    A bar chart showing monthly sales from January to June.
    Sales increased from 100 units in January to 250 units in June.
  </desc>
  <!-- Chart content -->
</svg>
```

#### 4. Use `aria-label` for Simple Cases
```xml
<svg role="img" aria-label="Star rating: 4 out of 5">
  <!-- Star icons -->
</svg>
```

#### 5. Hide Decorative SVG
For purely decorative graphics:

```xml
<svg aria-hidden="true" focusable="false">
  <!-- Decorative content -->
</svg>
```

### Complete Accessible Example
```xml
<svg width="200" height="200" viewBox="0 0 200 200" 
     role="img" aria-labelledby="iconTitle iconDesc">
  <title id="iconTitle">Success Icon</title>
  <desc id="iconDesc">
    A green circle with a white checkmark, indicating successful completion.
  </desc>
  <circle cx="100" cy="100" r="80" fill="#28a745"/>
  <polyline points="60,100 85,125 140,70" 
            fill="none" stroke="white" stroke-width="8" 
            stroke-linecap="round"/>
</svg>
```

---

## Best Practices

### 1. Always Use ViewBox for Scalable Graphics
```xml
<!-- Good: Scalable -->
<svg viewBox="0 0 100 100">
  <!-- content -->
</svg>

<!-- Less flexible: Fixed size only -->
<svg width="100" height="100">
  <!-- content -->
</svg>
```

### 2. Keep Coordinate System Simple
Use round numbers and simple scales (e.g., 0-100, 0-1000).

### 3. Optimize Path Data
- Remove unnecessary precision
- Use relative commands when appropriate (lowercase letters)
- Tools like SVGO can help optimize

### 4. Group Related Elements
```xml
<g id="icon-group" fill="blue">
  <circle cx="50" cy="50" r="40"/>
  <circle cx="150" cy="50" r="40"/>
</g>
```

### 5. Use Semantic Structure
```xml
<svg>
  <defs>
    <!-- Define reusable elements -->
    <symbol id="icon">
      <!-- icon content -->
    </symbol>
  </defs>
  <use href="#icon" x="10" y="10"/>
</svg>
```

### 6. Consider Performance
- Minimize number of path nodes
- Use CSS for styling when possible
- Avoid excessive nesting
- Reuse elements with `<use>`

### 7. Provide Fallbacks
```xml
<svg>
  <!-- SVG content -->
  <image src="fallback.png" alt="Fallback image"/>
</svg>
```

### 8. Test Across Browsers
Different browsers may render SVG slightly differently, especially:
- Text rendering
- Stroke-width scaling
- Filter effects

### 9. Namespace for Standalone SVG Files
```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <!-- content -->
</svg>
```

### 10. Keep Accessibility in Mind
- Always provide `<title>` for meaningful graphics
- Use `<desc>` for complex images
- Test with screen readers
- Ensure sufficient color contrast

---

## Common Pitfalls and Solutions

### Pitfall 1: SVG Not Scaling
**Problem**: SVG has fixed width/height, no viewBox
```xml
<svg width="100" height="100">
  <!-- Won't scale well -->
</svg>
```

**Solution**: Add viewBox
```xml
<svg width="100" height="100" viewBox="0 0 100 100">
  <!-- Scales properly now -->
</svg>
```

### Pitfall 2: Distorted Aspect Ratio
**Problem**: Width and height don't match viewBox aspect ratio

**Solution**: Use `preserveAspectRatio` or match ratios
```xml
<svg width="200" height="100" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet">
  <!-- Maintains aspect ratio, adds letterboxing if needed -->
</svg>
```

### Pitfall 3: Coordinates Outside ViewBox
**Problem**: Elements positioned outside the viewBox aren't visible

**Solution**: Ensure all content fits within viewBox or adjust viewBox size

### Pitfall 4: Missing Stroke on Lines
**Problem**: Line element has no visible stroke
```xml
<line x1="0" y1="0" x2="100" y2="100"/>
```

**Solution**: Add stroke attribute
```xml
<line x1="0" y1="0" x2="100" y2="100" stroke="black"/>
```

---

## Resources and Further Reading

- [MDN SVG Documentation](https://developer.mozilla.org/en-US/docs/Web/SVG)
- [SVG Specification (W3C)](https://www.w3.org/TR/SVG2/)
- [SVG Accessibility Guidelines (W3C)](https://www.w3.org/TR/svg-aam-1.0/)
- [CSS Tricks: A Complete Guide to SVG](https://css-tricks.com/lodge/svg/)
- [Can I Use SVG](https://caniuse.com/?search=svg) - Browser compatibility

---

## Summary

- **ViewBox** is crucial for scalable, responsive SVG graphics
- Use semantic coordinate systems (0-100 is often easiest)
- Always include accessibility features (`role`, `title`, `desc`)
- Combine `width="100%"` with `viewBox` for responsive designs
- Test across different screen sizes and browsers
- Keep paths optimized and simple
- Use CSS for styling when possible
- Group related elements for better organization

SVG is a powerful tool for creating scalable, accessible graphics on the web. By understanding viewBox, coordinate systems, and accessibility features, you can create graphics that look great and work for everyone.

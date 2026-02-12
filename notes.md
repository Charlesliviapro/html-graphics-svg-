# SVG Notes: Attributes and Scaling

## Table of Contents
- [Introduction to SVG](#introduction-to-svg)
- [Basic SVG Structure](#basic-svg-structure)
- [Essential SVG Attributes](#essential-svg-attributes)
- [Understanding viewBox and Scaling](#understanding-viewbox-and-scaling)
- [Common SVG Shapes](#common-svg-shapes)
- [Styling SVG Elements](#styling-svg-elements)
- [Best Practices](#best-practices)

---

## Introduction to SVG

**SVG (Scalable Vector Graphics)** is an XML-based vector image format for defining two-dimensional graphics. Unlike raster images (PNG, JPG), SVGs are resolution-independent and scale without losing quality.

### Key Advantages:
- **Scalable**: Looks crisp at any size or resolution
- **Small file size**: Especially for simple graphics
- **Editable**: Can be styled with CSS and manipulated with JavaScript
- **Accessible**: Can include semantic markup and ARIA attributes
- **SEO-friendly**: Text content is searchable

---

## Basic SVG Structure

```xml
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <!-- SVG content goes here -->
  <circle cx="100" cy="100" r="50" fill="blue"/>
</svg>
```

### Inline SVG vs External SVG
- **Inline SVG**: Embedded directly in HTML (better for styling, animation, and accessibility)
- **External SVG**: Referenced via `<img>` or `<object>` tags (better for reusability and caching)

---

## Essential SVG Attributes

### Container Attributes

#### `width` and `height`
Defines the display dimensions of the SVG canvas in the document.

```xml
<svg width="300" height="200">
  <!-- Content scaled to 300x200 pixels -->
</svg>
```

- Can use units: `px`, `em`, `rem`, `%`, `vw`, `vh`
- Without units, defaults to pixels
- If omitted, SVG takes up available space (use with `viewBox`)

#### `viewBox`
Defines the coordinate system and aspect ratio of the SVG content.

```xml
<svg viewBox="0 0 100 100">
  <!-- Coordinate system: 0-100 on both axes -->
</svg>
```

**Syntax**: `viewBox="min-x min-y width height"`
- `min-x`, `min-y`: Starting position of the viewport (usually 0 0)
- `width`, `height`: Internal coordinate system dimensions

**Key Point**: `viewBox` is independent of `width` and `height`. It defines the internal coordinate system, while `width`/`height` define the display size.

#### `preserveAspectRatio`
Controls how the SVG scales when the aspect ratio of the `viewBox` doesn't match the viewport.

```xml
<svg viewBox="0 0 100 50" preserveAspectRatio="xMidYMid meet">
  <!-- Content is centered and scaled to fit -->
</svg>
```

**Common values**:
- `xMidYMid meet`: Center and scale to fit (default)
- `xMidYMid slice`: Center and scale to fill (may crop)
- `none`: Stretch to fill (distorts aspect ratio)

#### `xmlns`
Declares the SVG namespace (required for standalone SVG files, optional for inline HTML5).

```xml
<svg xmlns="http://www.w3.org/2000/svg">
```

---

## Understanding viewBox and Scaling

### How viewBox Works

The `viewBox` defines a rectangular region in user space that maps to the bounds of the viewport.

#### Example 1: Basic Scaling
```xml
<!-- viewBox matches display size: 1:1 ratio -->
<svg width="100" height="100" viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="40"/>
  <!-- Circle appears at half size with 40px radius -->
</svg>

<!-- viewBox smaller than display: 2x zoom -->
<svg width="100" height="100" viewBox="0 0 50 50">
  <circle cx="25" cy="25" r="20"/>
  <!-- Same circle appears doubled in size -->
</svg>
```

#### Example 2: Responsive SVG
```xml
<!-- SVG scales responsively while maintaining aspect ratio -->
<svg width="100%" viewBox="0 0 200 100">
  <rect x="50" y="25" width="100" height="50" fill="blue"/>
  <!-- Rectangle scales with container -->
</svg>
```

#### Example 3: Cropping and Panning
```xml
<!-- Show only a portion of the content (zoom and crop) -->
<svg width="200" height="200" viewBox="50 50 100 100">
  <circle cx="100" cy="100" r="80" fill="red"/>
  <!-- Shows center portion of circle, cropped and zoomed -->
</svg>
```

### Scaling Strategies

#### 1. Fixed Size SVG
```xml
<svg width="200" height="200">
  <circle cx="100" cy="100" r="50"/>
</svg>
```
- Fixed display dimensions
- No `viewBox` needed for simple cases
- Useful for icons and small graphics

#### 2. Fluid Width, Fixed Aspect Ratio
```xml
<svg width="100%" viewBox="0 0 200 100">
  <rect x="0" y="0" width="200" height="100" fill="blue"/>
</svg>
```
- Scales horizontally with container
- Maintains aspect ratio
- Most common for responsive design

#### 3. Icon-Style Scaling
```xml
<svg viewBox="0 0 24 24" width="48" height="48">
  <!-- Designed on 24x24 grid, displayed at 48x48 -->
  <path d="M12 2L2 12h3v8h14v-8h3L12 2z"/>
</svg>
```
- Design at one size (e.g., 24×24)
- Display at any size
- Perfect for icon systems

---

## Common SVG Shapes

### Circle
```xml
<circle cx="50" cy="50" r="40" fill="blue"/>
```
- `cx`, `cy`: Center coordinates
- `r`: Radius

### Rectangle
```xml
<rect x="10" y="10" width="80" height="60" rx="5" fill="green"/>
```
- `x`, `y`: Top-left corner position
- `width`, `height`: Dimensions
- `rx`, `ry`: Corner radius (optional, for rounded corners)

### Ellipse
```xml
<ellipse cx="50" cy="50" rx="40" ry="25" fill="purple"/>
```
- `cx`, `cy`: Center coordinates
- `rx`: Horizontal radius
- `ry`: Vertical radius

### Line
```xml
<line x1="10" y1="10" x2="90" y2="90" stroke="black" stroke-width="2"/>
```
- `x1`, `y1`: Start point
- `x2`, `y2`: End point
- Requires `stroke` attribute (lines have no fill)

### Polyline
```xml
<polyline points="10,10 50,50 90,10" fill="none" stroke="red" stroke-width="2"/>
```
- `points`: Space or comma-separated coordinate pairs
- Creates connected line segments

### Polygon
```xml
<polygon points="50,10 90,90 10,90" fill="yellow" stroke="orange" stroke-width="2"/>
```
- Similar to polyline but automatically closes the shape
- Last point connects to first point

### Path
The most powerful and flexible shape element.

```xml
<path d="M 10 10 L 90 90 L 10 90 Z" fill="pink"/>
```

**Common Path Commands**:
- `M x y`: Move to (start new subpath)
- `L x y`: Line to
- `H x`: Horizontal line to
- `V y`: Vertical line to
- `C x1 y1 x2 y2 x y`: Cubic Bezier curve
- `Q x1 y1 x y`: Quadratic Bezier curve
- `A rx ry rotation large-arc sweep x y`: Elliptical arc
- `Z`: Close path

---

## Styling SVG Elements

### Presentation Attributes
Applied directly as XML attributes:

```xml
<circle cx="50" cy="50" r="40" 
        fill="#FF6B6B" 
        stroke="#333" 
        stroke-width="3"
        opacity="0.8"/>
```

### Common Presentation Attributes:
- `fill`: Fill color (color name, hex, rgb, rgba, or `none`)
- `stroke`: Stroke color
- `stroke-width`: Stroke thickness
- `stroke-linecap`: Line ending style (`butt`, `round`, `square`)
- `stroke-linejoin`: Corner style (`miter`, `round`, `bevel`)
- `stroke-dasharray`: Dashed line pattern
- `opacity`: Overall opacity (0-1)
- `fill-opacity`: Fill opacity only
- `stroke-opacity`: Stroke opacity only

### CSS Styling
SVG elements can be styled with CSS:

```css
.my-circle {
  fill: #4CAF50;
  stroke: #2E7D32;
  stroke-width: 3px;
  transition: fill 0.3s;
}

.my-circle:hover {
  fill: #66BB6A;
}
```

```xml
<circle class="my-circle" cx="50" cy="50" r="40"/>
```

---

## Best Practices

### 1. Use viewBox for Scalability
Always include a `viewBox` when you want responsive, scalable SVGs:
```xml
<svg viewBox="0 0 100 100" width="100%">
  <!-- Content here -->
</svg>
```

### 2. Optimize Path Data
Remove unnecessary precision and use relative commands:
```xml
<!-- Before -->
<path d="M 10.0000 10.0000 L 90.0000 90.0000"/>

<!-- After -->
<path d="M10 10L90 90"/>
```

### 3. Group Related Elements
Use `<g>` to apply attributes to multiple elements:
```xml
<g fill="blue" stroke="navy" stroke-width="2">
  <circle cx="50" cy="50" r="20"/>
  <circle cx="100" cy="50" r="20"/>
</g>
```

### 4. Use `<defs>` for Reusable Elements
Define gradients, patterns, and symbols once:
```xml
<svg>
  <defs>
    <linearGradient id="grad1">
      <stop offset="0%" style="stop-color:rgb(255,255,0)"/>
      <stop offset="100%" style="stop-color:rgb(255,0,0)"/>
    </linearGradient>
  </defs>
  <rect fill="url(#grad1)" x="10" y="10" width="80" height="80"/>
</svg>
```

### 5. Accessibility
Always consider accessibility:
```xml
<!-- Decorative -->
<svg aria-hidden="true" focusable="false">
  <!-- decorative content -->
</svg>

<!-- Informative -->
<svg role="img" aria-labelledby="title">
  <title id="title">Description of the image</title>
  <!-- meaningful content -->
</svg>
```

### 6. Inline vs External
- **Use inline SVG** when you need:
  - CSS styling and animations
  - JavaScript manipulation
  - Accessibility features
  - Small, few graphics

- **Use external SVG** when you need:
  - Caching across pages
  - Large or many graphics
  - Browser compatibility (older browsers)

### 7. Coordinate System Tips
- Design on a sensible grid (e.g., 100×100 or 24×24 for icons)
- Use whole numbers when possible for crisper rendering
- Consider the actual display size when choosing coordinate precision

### 8. Performance
- Simplify paths and reduce points
- Avoid excessive gradients and filters
- Use CSS transforms instead of manipulating coordinates
- Consider using `<use>` for repeated elements

---

## Quick Reference

### Responsive SVG Template
```xml
<svg viewBox="0 0 200 200" width="100%" xmlns="http://www.w3.org/2000/svg">
  <!-- Your scalable content here -->
</svg>
```

### Icon SVG Template
```xml
<svg viewBox="0 0 24 24" width="24" height="24" 
     role="img" aria-label="Icon description">
  <!-- Icon paths here -->
</svg>
```

### Accessible Informative SVG Template
```xml
<svg viewBox="0 0 200 100" role="img" 
     aria-labelledby="chart-title chart-desc">
  <title id="chart-title">Chart Title</title>
  <desc id="chart-desc">Detailed description of the chart data</desc>
  <!-- Chart content here -->
</svg>
```

---

## Resources

- [MDN SVG Documentation](https://developer.mozilla.org/en-US/docs/Web/SVG)
- [W3C SVG Specification](https://www.w3.org/TR/SVG2/)
- [SVG Accessibility Guidelines](https://www.w3.org/WAI/tutorials/images/)
- [CSS-Tricks SVG Guide](https://css-tricks.com/lodge/svg/)

---

*This guide covers the fundamentals of SVG for web development. For more advanced topics like animations, filters, and complex path operations, refer to the resources listed above.*

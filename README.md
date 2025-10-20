# Vuboi Design Token

Comprehensive design token system for Vuboi UI framework with structured naming conventions, organized token hierarchy, and multi-platform support.

## 📋 Table of Contents

- [Overview](#overview)
- [Token Structure](#token-structure)
- [Naming Convention](#naming-convention)
- [Token Categories](#token-categories)
- [Base Levels](#base-levels)
- [Modifier Levels](#modifier-levels)
- [Organization](#organization)
- [Getting Started](#getting-started)
- [Building & Output](#building--output)

---

## Overview

Vuboi Design Token is a centralized system for managing visual design decisions across all platforms and components. Following Nathan Curtis's naming conventions in design systems, our tokens are organized into logical hierarchies that improve team collaboration and reduce inconsistency.

### Key Features

✨ **Structured Hierarchy** - Organized token names that reflect design intent
🎨 **Multi-Mode Support** - Separate tokens for light and dark modes
📱 **Cross-Platform** - Support for web, mobile, and design tools
🔄 **Aliasing** - Reference tokens to reduce duplication
📚 **Comprehensive** - Covers colors, typography, spacing, shadows, gradients, and more

---

## Token Structure

Our token naming follows a layered approach combining:

1. **Base Levels** - Category, concept, and property
2. **Modifier Levels** - Variant, state, scale, and mode
3. **Object Levels** - Component-specific tokens
4. **Namespace Levels** - System identification

### Example Token Paths

```
colors.text.primary
├─ Category: colors
├─ Property: text
└─ Variant: primary

colors.background.success.secondary
├─ Category: colors
├─ Concept: success
├─ Property: background
└─ Variant: secondary

colors.effects.shadows.lg
├─ Category: colors
├─ Concept: effects
├─ Type: shadows
└─ Scale: lg

typography.text.sm.regular
├─ Category: typography
├─ Concept: text
├─ Scale: sm (small)
└─ Weight: regular
```

---

## Naming Convention

### Naming Principles

**Avoid Homonyms** - Choose clear, unambiguous terms (e.g., `font` instead of `type`)

**Flexibility vs Specificity** - Balance generic reusability with purposeful intent

```
✅ Specific:  colors.text.primary      // Clear intent
❌ Generic:   colors.primary           // Ambiguous usage
```

**Homogeneity Within, Heterogeneity Between** - Keep related concepts together while separating distinct purposes

**Completeness** - Include only necessary levels to describe intent

```
✅ Good:   colors.background.brand.solid
❌ Bad:    colors.background.brand.solid.default.on-light
```

### Naming Format

```
{category}.{concept?}.{property?}.{variant?}.{modifier?}
```

- **{category}** - REQUIRED: colors, typography, spacing, shadow, etc.
- **{concept}** - OPTIONAL: success, warning, error, brand, etc.
- **{property}** - OPTIONAL: text, background, border, fill, etc.
- **{variant}** - OPTIONAL: primary, secondary, tertiary, etc.
- **{modifier}** - OPTIONAL: hover, active, disabled, on-dark, etc.

---

## Token Categories

### Colors

**Location:** `colors/`

#### Sections:

- **colors.text** - Text color tokens
  - Variants: primary, secondary, tertiary, quaternary, disabled, placeholder, etc.
  - Examples: `colors.text.primary`, `colors.text.disabled.subtle`

- **colors.background** - Background color tokens (nested by concept)
  - Concepts: primary, secondary, brand, success, warning, error, disabled, etc.
  - Variants within concept: default, hover, alt, solid, solidHover, etc.
  - Examples: `colors.background.primary.default`, `colors.background.brand.solid`

- **colors.border** - Border color tokens
  - Variants: primary, secondary, brand, disabled, subtle, etc.
  - Examples: `colors.border.primary`, `colors.border.brand`

- **colors.foreground** - Foreground color tokens (nested by concept)
  - Concepts: primary, secondary, brand, success, warning, error, tertiary, quaternary, disabled, white
  - Sub-variants: default, primary, secondary, primaryAlt, secondaryAlt, secondaryHover, hover, etc.
  - Examples: `colors.foreground.brand.primary`, `colors.foreground.success.secondary`

#### Base Colors

- **colors.base** - Core primitive colors
  - white, black, and other base hues
  - Examples: `colors.base.white`, `colors.base.black`

#### Semantic Colors

- **colors.brand** - Brand color palette (with scales: 50, 100, 200, ..., 900)
- **colors.success** - Success feedback colors
- **colors.warning** - Warning feedback colors
- **colors.error** - Error feedback colors
- **colors.grayLightMode** - Grayscale for light mode
- **colors.grayDarkMode** - Grayscale for dark mode

#### Effects

- **colors.effects.shadows** - Shadow definitions (nested by size)
  - Sizes: xs, sm, md, lg, xl, 2xl, 3xl
  - Example: `colors.effects.shadows.lg`

- **colors.effects.shadowColors** - Shadow color references
  - Example: `colors.effects.shadowColors.shadowColor01`

- **colors.effects.focusRings** - Focus ring definitions
  - Examples: `colors.effects.focusRings.focusRing`, `colors.effects.focusRings.focusRingError`

- **colors.effects.gradients** - Gradient definitions (nested by type)
  - Types: skeuomorphic, gray, brand, linear
  - Examples: `colors.effects.gradients.brand.600To500Deg90`, `colors.effects.gradients.linear.02`

### Typography

**Location:** `typography.json`

- **fontFamily** - Font family definitions
  - display: Bricolage Grotesque (headings)
  - text: Inter (body text)

- **typography.display** - Display typography tokens (headings)
  - Sizes: xs, sm, md, lg, xl, 2xl
  - Weights: regular, medium, semibold, bold
  - Includes: fontSize, fontWeight, lineHeight, letterSpacing, etc.
  - Example: `typography.display.2xl.bold`

- **typography.text** - Body typography tokens
  - Sizes: xs, sm, md, lg, xl
  - Weights: regular, medium, semibold, bold
  - Includes: fontSize, fontWeight, lineHeight, letterSpacing, etc.
  - Example: `typography.text.md.regular`

### Spacing & Sizing

**Location:** `modes/light.json` and `modes/dark.json`

Spacing and sizing follow a modular scale system.

### Component-Specific Tokens

**Location:** `modes/light.json` and `modes/dark.json` → `colors.components`

Component-specific tokens are organized by component group:

- **toggles** - Toggle and switch components
- **mockups** - Mockup and preview components
- **buttons** - Button variants

Example: `colors.components.toggles.toggleBorder`

---

## Base Levels

### Category

Top-level classification of token type:

| Category | Purpose | Examples |
|----------|---------|----------|
| **colors** | Color palette | text, background, border, fill |
| **typography** | Font and text styling | font-family, font-size, font-weight |
| **spacing** | Dimensional values | padding, margin, gap |
| **shadow** | Depth effects | box-shadow definitions |
| **time** | Animation timing | duration, delay |

### Property

Describes how the category is applied:

**Color properties:**
- `text` - Foreground/text color
- `background` - Background color
- `border` - Border color
- `fill` - Fill/icon color

**Typography properties:**
- `size` - Font size
- `weight` - Font weight
- `line-height` - Line height
- `letter-spacing` - Letter spacing

### Concept

Groups related tokens by purpose:

**Feedback concepts (colors):**
- `success` - Positive/confirmation state
- `warning` - Caution/attention state
- `error` - Negative/alert state

**Action concepts (colors):**
- `brand` - Brand/primary action color
- `primary` - Default interactive element
- `secondary` - Alternative interactive element

**Typography concepts:**
- `display` - Large, prominent headings
- `text` - Body text and paragraphs

---

## Modifier Levels

### Variant

Distinguishes alternative use cases within a concept:

```
colors.text.primary
colors.text.secondary
colors.text.tertiary
colors.text.quaternary
```

**Common variants:**
- `primary` / `default` / `base` - Primary option
- `secondary` / `subdued` / `subtle` - Secondary option
- `tertiary` / `nonessential` - Tertiary option
- `quaternary` - Quaternary option

### State

Describes interactive states:

```
colors.background.primary.default    // Normal state
colors.background.primary.hover      // Pointer over element
colors.background.brand.solidHover   // Hovered solid variant
```

**Common states:**
- `default` - Normal/resting state
- `hover` - Pointer positioned above
- `active` / `pressed` - Element activated
- `focus` - Element can accept input
- `disabled` - Element not interactive
- `visited` - Link already visited

### Scale

Represents sizing choices with ordered or enumerated values:

```
// Enumerated (numbered scales)
typography.display.1
typography.display.2
typography.display.3

// Dimensional (size/space scales)
colors.effects.shadows.xs
colors.effects.shadows.sm
colors.effects.shadows.md
colors.effects.shadows.lg

// Proportion-based
spacing.1-x    // 1x base unit
spacing.2-x    // 2x base unit
spacing.4-x    // 4x base unit
```

**Scale types in Vuboi:**
- T-shirt sizes: `xs`, `sm`, `md`, `lg`, `xl`, `2xl`, `3xl`
- Numeric: `01`, `02`, `03` ... `10`
- Angle-based: `Deg90`, `Deg45`, `Deg27`

### Mode

Distinguishes values across different contexts (typically light/dark):

```
// Explicit mode naming
colors.text.primary              // Light mode (default)
colors.text.primary.on-dark      // Dark mode variant

// Applied in modes/
tokens/modes/light.json          // Light mode tokens
tokens/modes/dark.json           // Dark mode tokens
```

---

## Organization

### Directory Structure

```
tokens/
├── typography.json              # Font families, sizes, weights
├── colors.json                  # Base color definitions (if exists)
├── modes/
│   ├── light.json              # Light mode color tokens
│   └── dark.json               # Dark mode color tokens
└── README.md                    # This file
```

### Mode Files (light.json, dark.json)

Each mode file contains:

```json
{
  "colors": {
    "text": { ... },
    "background": { ... },
    "border": { ... },
    "foreground": { ... },
    "base": { ... },
    "brand": { ... },
    "success": { ... },
    "warning": { ... },
    "error": { ... },
    "grayLightMode": { ... },
    "grayDarkMode": { ... },
    "effects": {
      "shadows": { ... },
      "shadowColors": { ... },
      "focusRings": { ... },
      "gradients": { ... }
    },
    "components": { ... }
  }
}
```

### Token Nesting Strategy

Tokens use hierarchical nesting for related groups:

**Flat (when solo):**
```json
{
  "text": {
    "primary": { "value": "...", "type": "color" }
  }
}
```

**Nested (when grouped):**
```json
{
  "background": {
    "primary": {
      "default": { "value": "...", "type": "color" },
      "hover": { "value": "...", "type": "color" }
    },
    "brand": {
      "primary": { "value": "...", "type": "color" },
      "solid": { "value": "...", "type": "color" },
      "solidHover": { "value": "...", "type": "color" }
    }
  }
}
```

This results in token paths like:
- `colors.background.primary.default`
- `colors.background.brand.solid`
- `colors.background.brand.solidHover`

---

## Getting Started

### Prerequisites

- Node.js 14+
- npm or yarn

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/vuboi-token.git

# Install dependencies (if using Style Dictionary or build tools)
npm install
```

### Viewing Tokens

1. **In Design Tools:** Import tokens via Figma plugin or similar
2. **In Code:** Reference tokens in your application
3. **In Documentation:** Use Style Dictionary output

### Adding New Tokens

1. Identify the appropriate **mode file** (light.json or dark.json)
2. Find the relevant **category** (colors, typography, etc.)
3. Follow the **naming convention** for the new token
4. Add the token with `value` and `type` properties
5. Test references and ensure consistency

**Example: Adding a new text color**

```json
{
  "colors": {
    "text": {
      "newVariant": {
        "value": "#2D3748",
        "type": "color",
        "description": "New text variant for specific use case"
      }
    }
  }
}
```

---

## Building & Output

### Style Dictionary Configuration

Tokens are built using **Style Dictionary** for multi-platform support.

**Supported output formats:**

| Platform | Format | Output |
|----------|--------|--------|
| **Web** | CSS | CSS custom properties |
| **Web** | SCSS | SCSS variables & maps |
| **Web** | JavaScript | CommonJS & ESM modules |
| **Web** | TypeScript | TypeScript declarations |
| **Web** | JSON | Raw JSON |
| **Mobile** | Flutter | Dart class constants |
| **Mobile** | Compose | Kotlin object definitions |
| **Android** | Android XML | Android resource files |
| **iOS** | iOS | Swift constants |

### Build Commands

```bash
# Build all platforms
npm run build

# Build specific platform
npm run build:web
npm run build:mobile
npm run build:android
npm run build:ios

# Generate documentation
npm run build:docs

# Watch for changes
npm run build:watch
```

### Output Directory

```
build/
├── web/
│   ├── css/
│   ├── scss/
│   ├── js/
│   └── ts/
├── mobile/
│   ├── flutter/
│   └── compose/
├── android/
└── ios/
```

---

## Best Practices

### ✅ DO

- ✅ Use consistent naming across all tokens
- ✅ Group related tokens using nesting
- ✅ Document tokens with descriptions
- ✅ Reference tokens instead of hardcoding values
- ✅ Keep token names focused on intent, not implementation
- ✅ Use semantic names over generic ones
- ✅ Test token output on target platforms

### ❌ DON'T

- ❌ Use inconsistent naming patterns
- ❌ Create deeply nested structures (max 3-4 levels)
- ❌ Mix functional and semantic naming
- ❌ Create token names that duplicate their parent level
- ❌ Include color values in token names (e.g., `blue-button`)
- ❌ Prematurely globalize component-specific tokens
- ❌ Use homonyms in category names

---

## Examples

### Color Token Reference

```
colors.text.primary
colors.text.secondary
colors.text.disabled.subtle
colors.background.brand.solid
colors.background.brand.solidHover
colors.border.primary
colors.foreground.success.primary
colors.foreground.error.secondary
colors.effects.shadows.lg
colors.effects.gradients.brand.600To500Deg90
```

### Typography Token Reference

```
typography.text.sm.regular
typography.text.md.semibold
typography.display.xl.bold
typography.display.2xl.medium
```

### Using Tokens in Code

**CSS:**
```css
.button {
  color: var(--colors-text-primary);
  background-color: var(--colors-background-brand-solid);
}
```

**JavaScript:**
```javascript
import tokens from '@vuboi/tokens';

const buttonColor = tokens.colors.background.brand.solid;
```

**TypeScript:**
```typescript
import * as tokens from '@vuboi/tokens';

const textColor: string = tokens.colors.text.primary;
```

---

## Contributing

### Guidelines

1. Follow the established naming convention
2. Test tokens across all relevant platforms
3. Document any new token categories or concepts
4. Update this README if adding new structure
5. Ensure token references are valid and resolve correctly

### Versioning

Tokens follow semantic versioning. Major changes to naming or structure increment the major version.

---

## Resources

- [Nathan Curtis - Naming Tokens in Design Systems](https://medium.com/@nathancurtis)
- [Salesforce Lightning Design System Tokens](https://www.lightningdesignsystem.com)
- [Style Dictionary Documentation](https://amzn.github.io/style-dictionary)
- [Design Tokens Community Group](https://www.designtokens.org)

---

## License

Vuboi Design Token - All rights reserved

---

**Last Updated:** 2025-10-20
**Maintained by:** Vuboi Design System Team

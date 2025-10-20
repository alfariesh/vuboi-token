# Style Dictionary Configuration Guide

## Overview
File `config.json` mengatur bagaimana tokens akan ditransformasi dan dioutput ke berbagai format untuk berbagai platform.

---

## 📋 STRUKTUR CONFIG

### 1. **Source Files**
```json
"source": [
  "tokens/primitives.json",
  "tokens/typography.json",
  "tokens/radius.json",
  "tokens/widths.json",
  "tokens/containers.json",
  "tokens/spacing.json",
  "tokens/modes/**/*.json"
]
```

**Penjelasan:**
- Menentukan file token mana saja yang akan diproses
- `tokens/modes/**/*.json` menggunakan glob pattern untuk include semua file di folder modes

---

## 🎨 PLATFORMS & OUTPUT FORMATS

### 1. **CSS Platform** (`build/css/variables.css`)

```json
{
  "transformGroup": "css",
  "buildPath": "build/css/",
  "files": [{
    "destination": "variables.css",
    "format": "css/variables",
    "options": {
      "outputReferences": true,
      "prefix": "vuboi"
    }
  }]
}
```

**Transforms yang digunakan:**
- `name/camel` - Mengubah nama token ke camelCase
- `color/hex` - Konversi warna ke format HEX
- `size/px` - Mengkonversi size ke pixel
- `typography/css/shorthand` - Typography dalam format CSS shorthand

**Output contoh:**
```css
:root {
  --vuboi-colors-text-primary900: #181d27;
  --vuboi-spacing-lg: 12px;
}
```

**Kegunaan:**
- ✓ Web development (HTML/CSS)
- ✓ Paling umum untuk design system
- ✓ Browser support terbaik

---

### 2. **SCSS Platform** (`build/scss/`)

**File 1: `_variables.scss`**
```json
{
  "format": "scss/variables",
  "options": {
    "outputReferences": true,
    "prefix": "vuboi"
  }
}
```

**Output contoh:**
```scss
$vuboi-colors-text-primary900: #181d27;
$vuboi-spacing-lg: 12px;
```

**Kegunaan:**
- ✓ SCSS projects
- ✓ Kompatibel dengan CSS variables
- ✓ Dapat digunakan di mixin/functions

---

**File 2: `_maps.scss`**
```json
{
  "format": "scss/map-deep",
  "options": {
    "outputReferences": true
  }
}
```

**Output contoh:**
```scss
$tokens: (
  "colors": (
    "text": (
      "primary900": #181d27
    )
  ),
  "spacing": (
    "lg": 12px
  )
);
```

**Kegunaan:**
- ✓ Nested structure untuk akses via `map-get($tokens, 'colors', 'text', 'primary900')`
- ✓ Lebih terstruktur dan organized
- ✓ Mudah untuk looping dan dynamic styling

---

### 3. **JavaScript Platform** (`build/js/`)

**File 1: `tokens.js` (CommonJS)**
```json
{
  "format": "javascript/module",
  "options": {
    "outputReferences": true
  }
}
```

**Output contoh:**
```javascript
module.exports = {
  colors: {
    text: {
      primary900: "#181d27"
    }
  },
  spacing: {
    lg: "12px"
  }
};
```

**Kegunaan:**
- ✓ Node.js projects
- ✓ Webpack bundlers lama
- ✓ CommonJS environments

---

**File 2: `tokens.mjs` (ES Module)**
```json
{
  "format": "javascript/esm",
  "options": {
    "outputReferences": true
  }
}
```

**Output contoh:**
```javascript
export const tokens = {
  colors: {
    text: {
      primary900: "#181d27"
    }
  }
};

export default tokens;
```

**Kegunaan:**
- ✓ Modern JavaScript (ES6+)
- ✓ React, Vue, Next.js, Vite
- ✓ Tree-shakeable imports

---

### 4. **TypeScript Platform** (`build/ts/tokens.ts`)

```json
{
  "transformGroup": "typescript",
  "buildPath": "build/ts/",
  "files": [{
    "format": "typescript/es6-declarations",
    "options": {
      "outputReferences": true
    }
  }]
}
```

**Output contoh:**
```typescript
export const tokens: {
  colors: {
    text: {
      primary900: string;
    };
  };
  spacing: {
    lg: string;
  };
} = {
  colors: {
    text: {
      primary900: "#181d27"
    }
  },
  spacing: {
    lg: "12px"
  }
};
```

**Kegunaan:**
- ✓ TypeScript projects
- ✓ Type-safe token access
- ✓ Autocomplete di editor

---

### 5. **JSON Platform** (`build/json/tokens.json`)

```json
{
  "transformGroup": "js",
  "buildPath": "build/json/",
  "files": [{
    "destination": "tokens.json",
    "format": "json",
    "options": {
      "outputReferences": true
    }
  }]
}
```

**Output contoh:**
```json
{
  "colors": {
    "text": {
      "primary900": "#181d27"
    }
  },
  "spacing": {
    "lg": "12px"
  }
}
```

**Kegunaan:**
- ✓ Language-agnostic format
- ✓ API responses
- ✓ Design tools integration
- ✓ Fallback untuk tools yang tidak support format lain

---

## 🔄 OPTIONS EXPLANATION

### `outputReferences: true`
- Ketika ada token yang reference token lain, reference akan tetap dikompilasi
- Contoh: Jika `token.value = "{colors.text.primary}"`, akan output sebagai referensi yang resolved

### `prefix: "vuboi"`
- Hanya berlaku untuk CSS/SCSS
- Menambahkan prefix ke setiap variable name
- Contoh: `--vuboi-colors-text-primary900` bukan `--colors-text-primary900`
- Mencegah naming conflicts dengan library lain

---

## 📊 RECOMMENDED USAGE

| Use Case | Platform | Format |
|----------|----------|--------|
| Web (HTML/CSS) | CSS | `css/variables` |
| SCSS Projects | SCSS | `scss/variables` + `scss/map-deep` |
| React/Vue/Next.js | JavaScript | `javascript/esm` |
| TypeScript | TypeScript | `typescript/es6-declarations` |
| Other Languages | JSON | `json` |
| Design Tools API | JSON | `json` |

---

## 🚀 HOW TO BUILD

```bash
# Install Style Dictionary
npm install --save-dev style-dictionary

# Build all platforms
npx style-dictionary build

# Build specific platform
npx style-dictionary build --platform css
npx style-dictionary build --platform scss
npx style-dictionary build --platform javascript
```

---

## 📁 OUTPUT STRUCTURE

```
build/
├── css/
│   └── variables.css           # CSS custom properties
├── scss/
│   ├── _variables.scss         # SCSS variables
│   └── _maps.scss              # SCSS maps
├── js/
│   ├── tokens.js               # CommonJS export
│   └── tokens.mjs              # ES Module export
├── ts/
│   └── tokens.ts               # TypeScript declarations
└── json/
    └── tokens.json             # JSON format
```

---

## ✅ CHECKLIST

- ✓ Config supports multiple platforms
- ✓ Semantic token naming (camelCase)
- ✓ Color transformation to HEX format
- ✓ Size transformation to pixels
- ✓ Reference resolution enabled
- ✓ Prefix to prevent conflicts
- ✓ Ready for production use

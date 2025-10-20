# Transform & Format Selection Explanation

## 📍 PILIHAN YANG DIGUNAKAN DALAM CONFIG

### Transforms Selected (dari opsi yang tersedia)

| Transform | Kategori | Alasan Dipilih | Use Case |
|-----------|----------|---|----------|
| `name/camel` | Naming | ✓ Semantik dan standard untuk web | Semua output |
| `color/hex` | Color | ✓ Format paling kompatibel | Web browsers |
| `size/px` | Size | ✓ Standard untuk web CSS | Web layouts |
| `typography/css/shorthand` | Typography | ✓ Format CSS standard | CSS typography |
| `shadow/css/shorthand` | Shadow | ✓ Format CSS standard | Box-shadow properties |

---

### Formats Selected (dari opsi yang tersedia)

| Format | Kategori | Alasan Dipilih | Output Type |
|--------|----------|---|---|
| `css/variables` | CSS | ✓ Modern browser support | CSS Custom Properties (:root) |
| `scss/variables` | SCSS | ✓ Kompatibel dengan CSS variables | SCSS $variables |
| `scss/map-deep` | SCSS | ✓ Nested structure untuk map-get() | SCSS nested maps |
| `javascript/module` | JavaScript | ✓ CommonJS untuk Node.js | module.exports {} |
| `javascript/esm` | JavaScript | ✓ Modern JavaScript standard | export default {} |
| `typescript/es6-declarations` | TypeScript | ✓ Type-safe dengan declarations | TypeScript .d.ts style |
| `json` | JSON | ✓ Language-agnostic format | Raw JSON structure |

---

## ❌ PILIHAN YANG TIDAK DIGUNAKAN & ALASANNYA

### Transforms NOT Selected

| Transform | Alasan |
|-----------|--------|
| `name/kebab` | Sudah menggunakan camelCase di tokens |
| `name/pascal` | Tidak perlu untuk CSS |
| `name/constant` | Tidak perlu untuk dynamic design systems |
| `color/hsl` | HEX lebih universal untuk web |
| `color/rgb` | HEX lebih ringkas dan readable |
| `color/UIColor` | iOS specific, bukan priority |
| `size/rem` | Pixel lebih straightforward untuk token system |
| `size/sp` / `size/dp` | Android specific |
| `attribute/cti` | Tidak perlu untuk semantic tokens |
| `content/quote` | Tidak ada asset/content tokens |
| `time/seconds` | Tidak ada animation tokens di config |
| `cubicBezier/css` | Tidak ada easing tokens |
| `asset/*` | Tidak ada asset files |
| `html/icon` | Tidak ada icon system di config |

---

## 🎯 TRANSFORMGROUP MEANINGS

### What is `transformGroup`?

Setiap `transformGroup` adalah preset collection dari transforms yang saling kompatibel:

#### `transformGroup: "css"`
Transforms yang akan diterapkan:
```
name/camel              → camelCase naming
color/hex              → #rrggbb format
size/px                → pixels
typography/css/shorthand → CSS shorthand
shadow/css/shorthand   → box-shadow format
```

#### `transformGroup: "scss"`
Sama seperti CSS + SCSS-specific transforms:
```
(Semua dari CSS)
+ SCSS-specific naming rules
```

#### `transformGroup: "javascript"`
JavaScript-specific transforms:
```
name/camel
color/hex
size/px
```

#### `transformGroup: "typescript"`
TypeScript specific:
```
(Same as JavaScript + Type annotations)
```

---

## 🔍 DETAILED FORMAT COMPARISON

### CSS Variables vs Others

```css
/* css/variables */
:root {
  --vuboi-colors-text-primary: #181d27;
}
```

```scss
/* scss/variables */
$vuboi-colors-text-primary: #181d27;
```

```scss
/* scss/map-deep */
$tokens: (
  "colors": (
    "text": (
      "primary": #181d27
    )
  )
);
// Access: map-get($tokens, "colors", "text", "primary")
```

```javascript
/* javascript/module */
module.exports = {
  colors: {
    text: {
      primary: "#181d27"
    }
  }
};
```

```javascript
/* javascript/esm */
export const tokens = {
  colors: {
    text: {
      primary: "#181d27"
    }
  }
};
export default tokens;
```

---

## 💡 OPTIMAL CONFIGURATION RATIONALE

### Mengapa Konfigurasi Ini Dipilih?

1. **CSS Variables** (utama)
   - Sudah standard di modern browsers
   - Dapat di-override di runtime
   - Mendukung dark/light mode switching
   - Smallest file size

2. **SCSS** (secondary)
   - Untuk projects yang masih menggunakan SCSS
   - Nested maps untuk organized structure
   - Variables untuk backward compatibility

3. **JavaScript/TypeScript** (tertiary)
   - Untuk frontend frameworks (React, Vue, etc.)
   - Type safety dengan TypeScript
   - ESM untuk tree-shaking

4. **JSON** (fallback)
   - Universal format
   - Dapat dibaca oleh semua tools
   - API integration friendly

---

## 📦 ALTERNATIVE CONFIGURATIONS

### Jika hanya untuk React/Next.js:
```json
{
  "platforms": {
    "javascript": {
      "files": [{
        "format": "javascript/esm"
      }]
    }
  }
}
```

### Jika hanya untuk vanilla CSS:
```json
{
  "platforms": {
    "css": {
      "files": [{
        "format": "css/variables"
      }]
    }
  }
}
```

### Jika untuk Figma/Design Tools:
```json
{
  "platforms": {
    "json": {
      "files": [{
        "format": "json"
      }]
    }
  }
}
```

---

## ✨ BEST PRACTICES IMPLEMENTED

✓ Multiple output formats untuk flexibility
✓ Semantic naming (camelCase)
✓ Color references terselesaikan
✓ Standard size units (px)
✓ Reference resolution enabled
✓ Prefix untuk namespace safety
✓ Production-ready configuration

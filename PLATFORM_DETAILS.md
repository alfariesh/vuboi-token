# Additional Platforms Documentation

## 📱 FLUTTER PLATFORM

### Configuration
```json
{
  "transformGroup": "flutter",
  "buildPath": "build/flutter/",
  "files": [{
    "destination": "tokens.dart",
    "format": "flutter/class.dart",
    "className": "VuboiTokens"
  }]
}
```

### Output Example: `build/flutter/tokens.dart`
```dart
class VuboiTokens {
  // Colors
  static const Color colorsTextPrimary900 = Color(0xFF181d27);
  static const Color colorsTextWhite = Color(0xFFffffff);

  // Spacing
  static const double spacingXs = 4.0;
  static const double spacingMd = 8.0;
  static const double spacingLg = 12.0;

  // Border Radius
  static const double borderRadiusXs = 4.0;
  static const double borderRadiusMd = 8.0;

  // Typography
  static const TextStyle typographyTextXsRegular = TextStyle(
    fontWeight: FontWeight.w400,
    fontSize: 24.0,
    height: 18.0,
  );
}
```

### Usage in Flutter
```dart
import 'tokens.dart';

// Colors
Container(
  color: VuboiTokens.colorsTextPrimary900,
)

// Spacing
SizedBox(width: VuboiTokens.spacingMd)

// Border Radius
BorderRadius.circular(VuboiTokens.borderRadiusMd)

// Typography
Text(
  'Hello',
  style: VuboiTokens.typographyTextXsRegular,
)
```

### Key Benefits
✓ Type-safe token access
✓ No magic strings
✓ IDE autocomplete
✓ Easy to version control
✓ Direct Flutter integration

---

## 🎨 COMPOSE PLATFORM (Jetpack Compose)

### Configuration
```json
{
  "transformGroup": "compose",
  "buildPath": "build/android/",
  "files": [{
    "destination": "Theme.kt",
    "format": "compose/object",
    "objectName": "VuboiTokens",
    "package": "com.vuboi.tokens"
  }]
}
```

### Output Example: `build/android/Theme.kt`
```kotlin
package com.vuboi.tokens

object VuboiTokens {
    // Colors
    object Colors {
        val colorsTextPrimary900 = Color(0xFF181d27)
        val colorsTextWhite = Color(0xFFffffff)
    }

    // Spacing
    object Spacing {
        val spacingXs = 4.dp
        val spacingMd = 8.dp
        val spacingLg = 12.dp
    }

    // Typography
    object Typography {
        val typographyTextXsRegular = TextStyle(
            fontWeight = FontWeight.Normal,
            fontSize = 24.sp,
            lineHeight = 18.sp,
        )
    }
}
```

### Usage in Compose
```kotlin
import com.vuboi.tokens.VuboiTokens

Box(
    modifier = Modifier
        .background(VuboiTokens.Colors.colorsTextPrimary900)
        .padding(VuboiTokens.Spacing.spacingMd)
) {
    Text(
        "Hello",
        style = VuboiTokens.Typography.typographyTextXsRegular
    )
}
```

### Key Benefits
✓ Native Compose integration
✓ Type-safe with Kotlin
✓ Organized in objects
✓ Easy theming
✓ Modern Android development

---

## 🔧 ANDROID XML PLATFORM

### Configuration
```json
{
  "transformGroup": "android",
  "buildPath": "build/android/res/values/",
  "files": [
    {
      "destination": "tokens_colors.xml",
      "format": "android/colors"
    },
    {
      "destination": "tokens_dimens.xml",
      "format": "android/dimens"
    }
  ]
}
```

### Output Example 1: `build/android/res/values/tokens_colors.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<resources>
  <color name="colors_text_primary900">#181d27</color>
  <color name="colors_text_white">#ffffff</color>
  <color name="colors_border_brand">#9e77ed</color>
  <color name="colors_background_primary">#ffffff</color>
</resources>
```

### Output Example 2: `build/android/res/values/tokens_dimens.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<resources>
  <dimen name="spacing_xs">4dp</dimen>
  <dimen name="spacing_md">8dp</dimen>
  <dimen name="spacing_lg">12dp</dimen>
  <dimen name="border_radius_xs">4dp</dimen>
  <dimen name="border_radius_md">8dp</dimen>
  <dimen name="container_padding_mobile">16dp</dimen>
  <dimen name="container_padding_desktop">32dp</dimen>
</resources>
```

### Usage in Android (XML)
```xml
<!-- colors.xml -->
<color name="text_color">@color/colors_text_primary900</color>

<!-- activity_main.xml -->
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:padding="@dimen/spacing_md"
    android:background="@color/colors_background_primary"
/>
```

### Usage in Android (Java/Kotlin)
```kotlin
val textColor = ContextCompat.getColor(context, R.color.colors_text_primary900)
val padding = context.resources.getDimensionPixelSize(R.dimen.spacing_md)
```

### Key Benefits
✓ Standard Android XML format
✓ Works with legacy Android
✓ Easy to reference in layouts
✓ Runtime access in code
✓ Backward compatible

---

## 🍎 iOS SWIFT PLATFORM

### Configuration
```json
{
  "transformGroup": "ios",
  "buildPath": "build/ios/",
  "files": [{
    "destination": "VuboiTokens.swift",
    "format": "ios/macros"
  }]
}
```

### Output Example: `build/ios/VuboiTokens.swift`
```swift
import UIKit

// MARK: - Colors
let colorsTextPrimary900 = UIColor(red: 0.09, green: 0.11, blue: 0.15, alpha: 1.0)
let colorsTextWhite = UIColor(red: 1.0, green: 1.0, blue: 1.0, alpha: 1.0)
let colorsBorderBrand = UIColor(red: 0.62, green: 0.47, blue: 0.93, alpha: 1.0)

// MARK: - Spacing
let spacingXs = CGFloat(4.0)
let spacingMd = CGFloat(8.0)
let spacingLg = CGFloat(12.0)

// MARK: - Border Radius
let borderRadiusXs = CGFloat(4.0)
let borderRadiusMd = CGFloat(8.0)

// MARK: - Typography
let typographyTextXsRegular = UIFont(name: "Inter", size: 24.0)
```

### Usage in iOS (Swift)
```swift
import UIKit

class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()

        let label = UILabel()
        label.textColor = colorsTextPrimary900
        label.font = typographyTextXsRegular

        let button = UIButton()
        button.backgroundColor = colorsBorderBrand
        button.layer.cornerRadius = borderRadiusMd

        let view = UIView()
        view.widthAnchor.constraint(
            equalToConstant: containerMaxWidthDesktop
        ).isActive = true
    }
}
```

### Usage in iOS (SwiftUI)
```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack(spacing: spacingMd) {
            Text("Hello")
                .font(.system(size: typographyTextXsRegular.pointSize))
                .foregroundColor(Color(colorsTextPrimary900))

            Button("Click me") {
                // Action
            }
            .background(Color(colorsBorderBrand))
            .cornerRadius(borderRadiusMd)
        }
        .padding(spacingLg)
    }
}
```

### Key Benefits
✓ Native iOS integration
✓ UIKit + SwiftUI compatible
✓ CGFloat for precise sizing
✓ UIColor support
✓ Easy to maintain

---

## 📊 PLATFORM COMPARISON

| Platform | Language | Use Case | Output Format |
|----------|----------|----------|---|
| Flutter | Dart | Flutter apps | Dart class |
| Compose | Kotlin | Modern Android | Kotlin object |
| Android XML | XML/Java | Legacy/Modern Android | XML resources |
| iOS | Swift | iOS apps | Swift constants |
| CSS | CSS | Web | CSS variables |
| SCSS | SCSS | Web (SCSS) | SCSS variables |
| JavaScript | JS | Web/Node | JS object |
| TypeScript | TS | Web/Node | TS declarations |
| JSON | JSON | Universal | JSON object |

---

## 🚀 BUILD ALL PLATFORMS

```bash
# Build everything
npx style-dictionary build

# Build specific platform
npx style-dictionary build --platform flutter
npx style-dictionary build --platform compose
npx style-dictionary build --platform android
npx style-dictionary build --platform ios

# Or with wildcard
npx style-dictionary build --platform "flutter,compose,android,ios"
```

---

## 📁 COMPLETE OUTPUT STRUCTURE

```
build/
├── css/
│   └── variables.css
├── scss/
│   ├── _variables.scss
│   └── _maps.scss
├── js/
│   ├── tokens.js
│   └── tokens.mjs
├── ts/
│   └── tokens.ts
├── json/
│   └── tokens.json
├── flutter/
│   └── tokens.dart
├── android/
│   ├── Theme.kt (Compose)
│   └── res/values/
│       ├── tokens_colors.xml
│       └── tokens_dimens.xml
└── ios/
    └── VuboiTokens.swift
```

---

## ✅ CHECKLIST

- ✓ Flutter for mobile development
- ✓ Compose for modern Android
- ✓ Android XML for legacy Android
- ✓ iOS Swift for Apple platforms
- ✓ All major platforms covered
- ✓ Multiple format options per platform
- ✓ Production-ready configuration

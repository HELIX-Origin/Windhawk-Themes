---
layout: docs
parent: Documentation Directory
title: Glass Material Recipes
---

# Glass Material Recipes & Chrome Collapsing

Achieving a clean, high-performance translucent glass aesthetic in Windows 11 requires careful material selection and intentional chrome collapsing.

---

## 1. `WindhawkBlur` vs `AcrylicBrush`

| Feature | `WindhawkBlur` | `AcrylicBrush` |
|---|---|---|
| **Engine** | Windhawk DirectComposition / DWM shader | Native XAML compositor brush |
| **Blur Radius** | Direct pixel control (`BlurAmount="20"`) | Fixed system default |
| **Theme Resource** | Supported (`TintColor="{ThemeResource ...}"`) | Supported |
| **Flicker / Lag** | Smooth, fast, artifact-free | Can flicker during rapid redraws |
| **Best Used For** | **Primary panel surfaces** (main flyout panes) | **Interactive cards and hover fills** |

---

## 2. Recipe 1: Foundational Frosted Glass Panel

The base surface material for primary shell windows:

```yaml
styleConstants:
  - MySurfaceFill:=<WindhawkBlur BlurAmount="20" TintColor="{ThemeResource SystemChromeMediumColor}" TintOpacity="0.8"/>
  - MyRimBrush:=<LinearGradientBrush StartPoint="0,0" EndPoint="0,1"><GradientStop Color="#60808080" Offset="0.0"/><GradientStop Color="#40808080" Offset="1.0"/></LinearGradientBrush>
  - MyRimThickness=0,1,0,1
  - MyRadius=8            # illustrative — choose your own scale

controlStyles:
  - target: Grid#RootPanel
    styles:
      - Background:=$MySurfaceFill
      - BorderBrush:=$MyRimBrush
      - BorderThickness=$MyRimThickness
      - CornerRadius=$MyRadius
```

---

## 3. Recipe 2: Specular Rim Lighting

A vertical linear gradient border simulates natural ambient light catching the polished rim of a glass pane:

```xml
<LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
  <GradientStop Color="#60808080" Offset="0.0"/>
  <GradientStop Color="#40808080" Offset="1.0"/>
</LinearGradientBrush>
```

Paired with asymmetric thickness (`0,1,0,1`), the top and bottom borders appear highlighted while the side edges remain subtle.

---

## 4. Recipe 3: Collapsing Native Chrome & Shadows

To allow custom frosted glass to show through without muddy halos or black boxes:

```yaml
controlStyles:
  # Collapse native drop shadows
  - target: Selector#MasterFrame
    styles:
      - Shadow:=
      - BorderThickness=0

  # Collapse native opaque system background
  - target: Border#NativeBackground
    styles:
      - Background:=Transparent
```

---

## 5. Recipe 4: Interactive Glass Cards (Layered Depth)

Layered elements (quick settings tiles, calendar cells, flyout pills) sit above the base panel:

```yaml
styleConstants:
  - MySurfaceFill:=<AcrylicBrush TintColor="{ThemeResource SystemAltLowColor}" TintOpacity="0.4" FallbackColor="#202020"/>
  - MyOverlay:=<SolidColorBrush Color="#20ffffff"/>
  - MyOverlayAlt:=<SolidColorBrush Color="#40ffffff"/>
  - MyRadius=8            # illustrative — choose your own scale

controlStyles:
  - target: Border#CardRoot
    styles:
      - Background:=$MySurfaceFill
      - Background@PointerOver:=$MyOverlayAlt
      - Background@Pressed:=$MyOverlay
      - CornerRadius=$MyRadius
```

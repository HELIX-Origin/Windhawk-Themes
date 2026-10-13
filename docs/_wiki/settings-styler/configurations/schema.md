---
parent: Settings Styler
grand_parent: Target & Configuration Wiki
layout: wiki
title: Settings Configurations
---

# Settings Styler Configuration Schema & Options

Complete technical reference for top-level YAML configuration directives, token mechanics, and WindhawkBlur applications supported by `windows-11-settings-styler`.

---

## 1. Complete Top-Level Schema Definition

```yaml
theme: ''                              # Built-in preset theme name (empty string for custom suite theme)

styleConstants:                        # Shared token definitions and XAML object brushes
  - TokenName=ScalarValue              # Scalar tokens (e.g., margins, radii, dimensions)
  - TokenName:=<XAML Object>           # Complex XAML object brushes (WindhawkBlur, LinearGradientBrush)

themeResourceVariables:                # Global theme resource brush replacements across SystemSettings.exe
  - variableKey: ResourceKeyName
    value: "{ThemeResource ...}"

controlStyles:                         # Target selectors and styling rules applied to visual tree nodes
  - target: SelectorExpression
    styles:
      - Property=ScalarValue
      - Property:=<InlineXAML>
      - Property@VisualState=Value
      - Property:=                     # Empty value clears/resets the property
```

---

## 2. Deep Dive: `styleConstants` Token Mechanics

### Illustrative `styleConstants` Examples

> [!NOTE]
> The names and values below are placeholders — define your own tokens. What matters is the shape of each declaration.

```yaml
styleConstants:
  # Blur & Glass Foundations
  - MyBlur=<WindhawkBlur BlurAmount="20" TintColor="#10808080"/>
  - MySoftFill=<WindhawkBlur BlurAmount="8" TintColor="{ThemeResource SystemChromeMediumColor}" TintOpacity="0.7" />
  - MySurfaceFill=<WindhawkBlur BlurAmount="20" TintColor="{ThemeResource SystemChromeMediumColor}" TintOpacity="0.7" />
  - MyOverlay=<WindhawkBlur BlurAmount="30" TintColor="{ThemeResource SystemChromeMediumColor}" TintOpacity="0.8" />
  - MyBackground=$MySurfaceFill
  
  # Border Lighting & Accents
  - MyRimBrush=<LinearGradientBrush StartPoint="0,0" EndPoint="0,1"><GradientStop Color="#60808080" Offset="0.0" /><GradientStop Color="#40808080" Offset="1" /></LinearGradientBrush>
  - MyOverlayFill=<AcrylicBrush TintColor="{ThemeResource SystemAltLowColor}" TintOpacity="1" TintLuminosityOpacity="0.8" FallbackColor="{ThemeResource CardStrokeColorDefaultSolid}" />
  - MyAccentFill=<AcrylicBrush TintColor="{ThemeResource SystemAccentColor}" TintOpacity="1" TintLuminosityOpacity="0.5" FallbackColor="{ThemeResource CardStrokeColorDefaultSolid}" />
  - MyElementFill=<SolidColorBrush Color="{ThemeResource SystemAltLowColor}" Opacity="0.25" />
  
  # Geometric Scale
  - MyRimThickness=0,1,0,1
  - MyRadius=8           # Cards
  - MyRadiusSmall=4      # Action buttons, dropdowns, navigation pills
```

---

## 3. Key Recipes: Applying Frosted Glass to Settings

### Illustrative `styleConstants` Examples

> [!NOTE]
> The names and values below are placeholders — define your own tokens. What matters is the shape of each declaration.

```yaml
styleConstants:
  - MyBlur=<WindhawkBlur BlurAmount="20" TintColor="{ThemeResource SystemChromeMediumColor}" TintOpacity="0.7" />
  - MySurfaceFill=$MyBlur
  - MyRimBrush=<LinearGradientBrush StartPoint="0,0" EndPoint="0,1"><GradientStop Color="#60808080" Offset="0.0" /><GradientStop Color="#40808080" Offset="1" /></LinearGradientBrush>
  - MyRimThickness=0,1,0,1
  - MyRadius=8
  - MyRadiusSmall=4

controlStyles:
  # Whole-app window background
  - target: Page#RootPage
    styles:
      - Background:=$MyBackground

  # Transparent sidebar pane
  - target: SplitViewPane
    styles:
      - Background:=Transparent

  # Glass setting cards
  - target: SettingCard@CommonStates > Grid > Border#CardBackground
    styles:
      - Background:=$MyElementFill
      - BorderBrush:=$MyRimBrush
      - BorderThickness=$MyRimThickness
      - CornerRadius=$MyRadius

  # Glass setting expanders
  - target: SettingExpander@CommonStates > Grid > Border#HeaderBackground
    styles:
      - Background:=$MyElementFill
      - BorderBrush:=$MyRimBrush
      - BorderThickness=$MyRimThickness
      - CornerRadius=$MyRadius
```

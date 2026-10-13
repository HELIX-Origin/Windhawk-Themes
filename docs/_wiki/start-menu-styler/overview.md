---
layout: wiki
title: Start Menu Styler
parent: Target & Configuration Wiki
---

# Wiki: Windows 11 Start Menu Styler

Comprehensive development and styling guide for the **Windows 11 Start Menu Styler** mod (`windows-11-start-menu-styler`), covering architecture, visual tree hierarchy, token systems, and companion integrations.

---

## 1. Mod Overview & Process Host Architecture

| Specification Attribute | Detail | Technical Notes |
|---|---|---|
| **Windhawk Mod** | Start Menu Styler | Official Windhawk repository mod |
| **Target Process** | `StartMenuExperienceHost.exe` | Isolated UWP app package hosting the Start flyout |
| **Secondary Process** | `SearchHost.exe`<br>`SearchApp.exe` | Handles search input suggestions and WebView2 web views |
| **XAML Framework** | `Windows.UI.Xaml` | Standard UWP runtime (WinUI 2.8+ primitives) |
| **Windhawk Mod Floor** | `v1.7+` | Direct `WindhawkBlur` composition support introduced in v1.2+ |
| **Live Reload Capability** | Supported | Configuration changes apply dynamically without restarting the host |

```mermaid
flowchart LR
    Explorer["explorer.exe\n(Shell Controller)"] -->|Invokes via Win+S / Win Key| StartHost["StartMenuExperienceHost.exe\n(Target Process)"]
    StartHost --> WinUI["Windows.UI.Xaml Engine\n(UWP App Model)"]
    StartHost --> Search["SearchHost.exe\n(WebView2 Content)"]
    WindhawkMod["windows-11-start-menu-styler\n(Windhawk Engine)"] -->|Injects & Hooks| StartHost
```

---

## 2. Detailed Wiki Subsections

For in-depth reference documentation, consult the dedicated subcategories:

* 🎯 **[Visual Tree Element Targets](targets/elements.md)**: Exhaustive catalog of every verified root frame, search header, pinned grid, recommendations, bottom navigation pane, and Phone Link companion target.
* ⚙️ **[Configuration Schema & Token Directives](configurations/schema.md)**: Complete specification of YAML top-level keys (`styleConstants`, `themeResourceVariables`, `controlStyles`, `webContentStyles`), property operators, and XAML material definitions.

---

## 3. High-Level Hierarchy & Key Control Anchors

Understanding the structural hierarchy of `StartMenuExperienceHost.exe` is essential for crafting stable themes:

1. **Root Dismiss & Framing**:
   * `Border#DropShadowDismissTarget`: The outermost window host. It is collapsed (`Visibility=1`) to remove the system dark silhouette; a glass panel is instead rendered on `Grid#Root > Border` by styling its `Background`, `BorderBrush`, `BorderThickness`, and `CornerRadius` properties.
   * `Border#AcrylicBorder`: The default system acrylic plate. The main-menu acrylic (`Grid#MainMenu > Border#AcrylicBorder`) is collapsed (`Visibility=1`), while the companion-pane acrylic (`Grid#CompanionRoot > Border#AcrylicBorder`) is styled through its `Background` and `CornerRadius` properties.
2. **Two-Tone Acrylic Splitting**:
   * Windows 11 features a split background where the lower section (`Border#AcrylicOverlay`) has a darker tint. For a seamless single-panel aesthetic, this overlay can be collapsed (`Visibility=Collapsed`).
3. **Embedded Search Pill**:
   * Handled by `StartMenu.SearchBoxToggleButton`. Can be styled as a floating glass pill (set its `CornerRadius`) with rim lighting and subtle hover illumination.
4. **Phone Link Companion Expansion**:
   * `StartDocked.StartMenuCompanion#RightCompanion > Grid#CompanionRoot`: Expanding cards docked to the right of the Start Menu. Asymmetric corner radii allow it to join seamlessly with the main panel.

---

## 4. Key Recipes: Applying Frosted Glass Styling

Here is an architectural example demonstrating how to configure frosted glass foundations:

> [!NOTE]
> Names and values below are placeholders — define your own tokens.

```yaml
styleConstants:
  - MyBlur=<WindhawkBlur BlurAmount="20" TintColor="{ThemeResource SystemChromeMediumColor}" TintOpacity="0.7" />
  - MySurfaceFill=$MyBlur
  - MyRimBrush=<LinearGradientBrush StartPoint="0,0" EndPoint="0,1"><GradientStop Color="#60808080" Offset="0.0" /><GradientStop Color="#40808080" Offset="1" /></LinearGradientBrush>
  - MyRimThickness=0,1,0,1
  - MyRadius=8            # illustrative — choose your own scale
  - MyRadiusLarge=16

controlStyles:
  # Glass main panel
  - target: Grid#Root > Border
    styles:
      - Background:=$MySurfaceFill
      - BorderBrush:=$MyRimBrush
      - BorderThickness=$MyRimThickness
      - CornerRadius=$MyRadius

  # Collapse native drop shadow and dismiss host
  - target: Border#DropShadowDismissTarget
    styles:
      - Visibility=1

  # Collapse secondary system shadow layers
  - target: Border#StartDropShadow, Border#RightCompanionDropShadow, Border#RootGridDropShadow
    styles:
      - Visibility=Collapsed

  # Collapse main-menu acrylic plate (companion acrylic is styled separately)
  - target: Grid#MainMenu > Border#AcrylicBorder
    styles:
      - Visibility=1

  # Eliminate two-tone split overlay
  - target: Border#AcrylicOverlay
    styles:
      - Visibility=Collapsed
```

---

## 5. Companion Mod Integration

* **Shell Flyout Positions (`shell-flyout-positions`)**: Regulates the vertical offset (`offsetY: 12`) and alignment of the Start Menu window above the taskbar.
* **Start Button Colorizer (`start-button-colorizer`)**: Synchronizes the taskbar Start icon glyph with the Windows system accent color.

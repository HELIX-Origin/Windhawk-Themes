---
layout: wiki
title: Taskbar Styler
parent: Target & Configuration Wiki
---

# Wiki: Windows 11 Taskbar Styler

Comprehensive development and styling guide for the **Windows 11 Taskbar Styler** mod (`windows-11-taskbar-styler`) and integrated companion desktop enhancements.

---

## 1. Mod Overview & Architecture

| Specification Attribute | Detail | Technical Notes |
|---|---|---|
| **Windhawk Mod** | Taskbar Styler | Official Windhawk repository mod |
| **Target Process** | `explorer.exe` | Main Windows shell process hosting the taskbar |
| **XAML Framework** | `Microsoft.UI.Xaml` | Modern WinUI 3 XAML island runtime |
| **Windhawk Mod Floor** | `v1.10+` | DirectComposition glass and visual state injection |
| **Live Reload Capability** | Supported | Direct updates apply live to the active shell |

```mermaid
flowchart LR
    Explorer["explorer.exe\n(Host Process)"] --> Island["WinUI 3 XAML Island\n(Microsoft.UI.Xaml)"]
    Island --> Dock["Taskbar.TaskbarFrame\n(Primary Dock)"]
    Island --> SystemTray["SystemTray#SystemTray\n(Corner Status Area)"]
    WindhawkMod["windows-11-taskbar-styler\n(Windhawk Hook)"] -->|Injects Styles & Tokens| Island
```

---

## 2. Detailed Wiki Subsections

For exhaustive technical reference documentation, explore the dedicated subcategories:

* 🎯 **[Visual Tree Element Targets](targets/elements.md)**: Exhaustive catalog of every verified taskbar dock frame, task list button panel, active indicator pill, search box, system tray element, and snap assist flyout.
* ⚙️ **[Configuration Schema & Token Directives](configurations/schema.md)**: Complete specification of YAML top-level keys (`styleConstants`, `themeResourceVariables`, `controlStyles`, `clickThroughTaskbar`), property operators, and XAML material definitions.
* 🧩 **[Companion Mods & Settings Guide](companions/settings.md)**: Complete settings matrices for Taskbar Clock Customization, Taskbar Tray and Icon Tweaks, Dynamic Island for Windows, and Start Button Colorizer.

---

## 3. High-Level Hierarchy & Key Control Anchors

1. **Taskbar Glass Background**:
   * `Taskbar.TaskbarBackground#HoverFlyoutBackgroundControl`: Applying the `Background`, `BorderBrush`, `BorderThickness`, and `CornerRadius` properties renders a frosted glass surface with rim lighting directly on the taskbar background. The taskbar remains flush with the display edges.
   * `Rectangle#BackgroundFill` and `Rectangle#BackgroundStroke`: The native solid color fill and top border stroke. Both are collapsed (`Visibility=1`) to reveal the custom frosted blur foundation underneath.
2. **App Button Visual State Matrix**:
   * Running apps are represented by `Taskbar.TaskListButtonPanel`. The `@CommonStates` visual state group can be targeted to deliver custom state lighting across `ActiveNormal`, `ActivePointerOver`, `ActivePressed`, `InactiveNormal`, `InactivePointerOver`, and `MultiWindow` variants.
3. **Corner System Tray & Clock**:
   * `StackPanel#SystemTrayFrameGrid`: Can be sized (set its `Height`) and rendered transparent (`Background:=Transparent`, `BorderBrush:=Transparent`) with its native shadow removed (`Shadow:=`) to sit flush within the glass taskbar surface.
   * `Button#ShowDesktopButton`: The 1px edge strip can be collapsed or compressed to prevent accidental desktop reveals.

---

## 4. Key Recipes: Applying Taskbar Glass

> [!NOTE]
> Names and values below are placeholders — define your own tokens.

```yaml
styleConstants:
  - MyBlur=<WindhawkBlur BlurAmount="20" TintColor="{ThemeResource SystemChromeMediumColor}" TintOpacity="0.7" />
  - MySurfaceFill=$MyBlur
  - MyRimBrush=<LinearGradientBrush StartPoint="0,0" EndPoint="0,1"><GradientStop Color="#60808080" Offset="0.0" /><GradientStop Color="#40808080" Offset="1" /></LinearGradientBrush>
  - MyRimThickness=0,1,0,1
  - MyRadius=8            # illustrative — choose your own scale

controlStyles:
  # Taskbar glass background surface
  - target: Taskbar.TaskbarBackground#HoverFlyoutBackgroundControl
    styles:
      - Background:=$MySurfaceFill
      - BorderBrush:=$MyRimBrush
      - BorderThickness=$MyRimThickness
      - CornerRadius=$MyRadius

  # Collapse native solid color fill and top stroke
  - target: Taskbar.TaskbarFrame > Grid#RootGrid > Taskbar.TaskbarBackground > Grid > Rectangle#BackgroundFill
    styles:
      - Visibility=1
  - target: Rectangle#BackgroundStroke
    styles:
      - Visibility=1
```

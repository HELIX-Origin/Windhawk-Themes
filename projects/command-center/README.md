# Windhawk Command Center Suite – Base Styles

This directory contains the core Windhawk Styler mod configurations that implement the **"Command Center Glass"** unified dark/light frosted-glass design language for Windows 11.

> **Note:** The File Explorer styler is **deferred** (Milestone M.03) — no styler file exists in this directory. The mod remains approved under Rule 01's four-mod boundary, but the file was generated and then discontinued (git `1cc49e9`) due to a lack of plausible customizations (existing styles too similar; no real benefit yet). See the [File Explorer](#4-windows-11-file-explorer-styler-yml-deferred) section.

## Table of Contents

- [Overview](#overview)
- [Design Philosophy: "Command Center Glass"](#design-philosophy-command-center-glass)
- [Canonical Design Tokens](#canonical-design-tokens)
  - [Blur Materials](#blur-materials)
  - [Brushes & Colors](#brushes--colors)
  - [Radii Scale](#radii-scale)
  - [Layout & Sizing](#layout--sizing)
- [Base Mods](#base-mods)
  - [1. windows-11-taskbar-styler.yml](#1-windows-11-taskbar-styler-yml)
  - [2. windows-11-start-menu-styler.yml](#2-windows-11-start-menu-styler-yml)
  - [3. windows-11-notification-center-styler.yml](#3-windows-11-notification-center-styler-yml)
  - [4. windows-11-file-explorer-styler.yml (deferred)](#4-windows-11-file-explorer-styler-yml-deferred)
- [Style Constants Reference](#style-constants-reference)
- [Installation & Usage](#installation--usage)
- [Validation & Quality Gates](#validation--quality-gates)
- [Conventions & Compliance](#conventions--compliance)
- [Related Documentation](#related-documentation)

## Overview

The suite is approved for exactly **four** official Windhawk Styler mods — **three active styler files** exist here (one YAML per active mod); File Explorer is deferred. Each file uses Windhawk's Styler syntax (`styleConstants` + `controlStyles` with target selectors and per-state styles) to restyle the targeted shell surface with a consistent frosted-glass treatment.

| Styler File | Windhawk Mod ID | Target Process | Framework | Status |
|---|---|---|---|---|
| `windows-11-taskbar-styler.yml` | `windows-11-taskbar-styler` | `explorer.exe` | WinUI 3 / XAML | ✅ Shipped reference |
| `windows-11-start-menu-styler.yml` | `windows-11-start-menu-styler` | `StartMenuExperienceHost.exe` (LockApp surfaces also targeted) | UWP / WinUI 2 | ✅ Shipped reference |
| `windows-11-notification-center-styler.yml` | `windows-11-notification-center-styler` | `ShellExperienceHost.exe` / `ShellHost.exe` | UWP `Windows.UI.Xaml` | ✅ Generated & verified (polish active — M.02b / W.04) |
| *— (no styler file)* | `windows-11-file-explorer-styler` | `explorer.exe` | WinUI 3 `Microsoft.UI.Xaml` | ⏸️ Deferred (Milestone M.03) |

All active surfaces share the same canonical tokens and materials to preserve a unified "Command Center" look across taskbar, Start menu/lock widgets, and Notification Center/Quick Settings.

## Design Philosophy: "Command Center Glass"

- **Unified Frosted Blur**: Consistent `WindhawkBlur` (base `BlurAmount="20"`, tinted via `{ThemeResource SystemChromeMediumColor}` with `TintOpacity="0.7"`) forms the foundation glass surface across shells.
- **Top-Lit Glass Rim**: Signature vertical gradient border `LinearGradientBrush` from `#60808080` (top) → `#50404040` (mid) → `#40808080` (bottom) with `BorderThickness="0.3,1,0.3,1"`.
- **Harmonized Radii Scale**: XL (35) for Start Menu frames, L (25) for the Start search pill / (20) for taskbar confirmators, M (15 hover / 13 Notification Center panels) for groups/cards, S (10) for tiles/buttons/tooltips, XS (6) for chips/flyouts/menus.
- **Intentional Glass Layering**: Frosted foundation surfaces host layered glassy cards, pills, tiles and subtle element backgrounds (`ElementBackground` with low opacity) to create depth/contrast and visual hierarchy.
- **Minimal Native Chrome**: Native opaque fills, hard drop shadows, and heavy acrylic overlays are collapsed/hidden where they conflict with the glass treatment (e.g. taskbar `BackgroundFill`/`BackgroundStroke` and Start acrylic borders/overlays collapsed, drop shadows removed on Start/NC panels).
- **Theme-Aware**: Uses system theme resources (`SystemChromeMediumColor`, `SystemAltLowColor`, `SystemAccentColor`, `CardStrokeColorDefaultSolid`) with AcrylicBrush fallbacks to remain readable in light/dark.

## Canonical Design Tokens

### Blur Materials

| Token | Definition | Usage |
|---|---|---|
| `Translucent` | `WindhawkBlur BlurAmount="15" TintColor="#10808080"` | Subtle/translucent fills where lighter blur is desired |
| `Glass` | `WindhawkBlur BlurAmount="5" TintColor="{ThemeResource SystemChromeMediumColor}" TintOpacity="0.7"` | Very light glass accents |
| `Frosted` | `WindhawkBlur BlurAmount="20" TintColor="{ThemeResource SystemChromeMediumColor}" TintOpacity="0.7"` | **Base background** (`Background = $Frosted`) |
| `Acrylic` | `WindhawkBlur BlurAmount="30" TintColor="{ThemeResource SystemChromeMediumColor}" TintOpacity="0.8"` | Stronger blur for heavier surfaces/accents |

### Brushes & Colors

| Token | Definition | File(s) | Notes |
|---|---|---|---|
| `Background` | `$Frosted` | All | Canonical panel background material |
| `BorderBrush` | Vertical `LinearGradientBrush` (0,0)-(0,1): `#60808080` @0.0 → `#50404040` @0.25 → `#40808080` @1.0 | All | Top-lit glass rim |
| `BorderBrush2` | `WindhawkBlur BlurAmount="10" TintColor="#909090" TintOpacity="0.3"` | Start, NC | Alternate soft border treatment |
| `OverlayColor` | `AcrylicBrush TintColor="{ThemeResource SystemAltLowColor}" TintOpacity="1" TintLuminosityOpacity="0.8" FallbackColor="{ThemeResource CardStrokeColorDefaultSolid}"` | Taskbar, NC | Pointer-over/active state fill |
| `OverlayColor2` | Same as `OverlayColor` but `TintLuminosityOpacity="0.5"` | Taskbar, NC | Inactive/multi-window or alternate hover states |
| `AccentColor` | `AcrylicBrush TintColor="{ThemeResource SystemAccentColor}" TintOpacity="1" TintLuminosityOpacity="0.5" FallbackColor="{ThemeResource CardStrokeColorDefaultSolid}"` | Taskbar, NC | Accent-driven highlights (checked toggles, active running indicator) |
| `ActiveColor` | `AcrylicBrush TintColor="{ThemeResource SystemAltLowColor}" TintOpacity="1" TintLuminosityOpacity="1" FallbackColor="{ThemeResource CardStrokeColorDefaultSolid}"` | Taskbar | Opaque active-state pill/indicator fill |
| `ElementBackground` | `SolidColorBrush Color="{ThemeResource SystemAltLowColor}" Opacity="0.25"` | NC | Layered inner cards/panels (calendar, control center pages, toasts, etc.) |
| `ClockBG` | `SolidColorBrush Color="{ThemeResource SystemAccentColor}" Opacity="1"` | Start | Lock screen clock/date accent color |
| `BorderThickness` | `0.3,1,0.3,1` | All | Thin top/bottom, slightly thicker sides (rim) |

### Radii Scale

| File | Token | Value | Applied To |
|---|---|---|---|
| Start Menu | `CornerRadius` | `35` | XL – top-level Start/Menu frames (`Grid#Root > Border`, widget/media frames, companion) |
| Start Menu | `SearchBoxRadius` | `25` | L – Start search pill |
| Start Menu | `HoverCornerRadius` | `15` | M – hover surfaces/cards (declared in the scale) |
| Start Menu | `ElementCornerRadius` | `10` | S – buttons/tiles/list rows/tooltips |
| Start Menu | `FlyoutCornerRadius` | `6` | XS – flyouts, menus, jump/overflow flyouts |
| Taskbar | `R1` | `6` | XS/S – taskbar glass frame, button pills, icons, flyouts |
| Taskbar | `R2` | `20` | L – volume/brightness confirmators, badges |
| Notification Center | `PanelRadius` | `13` | L – Notification Center/Calendar/Control Center/main media panels |
| Notification Center | `CardRadius` | `10` | S – cards, toasts, toggle buttons, list content |
| Notification Center | `ChipRadius` | `6` | XS – chips, flyout presenters, jump-list items, footer buttons |

> **Note:** Start Menu's `HoverCornerRadius` (`15`) is declared in `styleConstants` as the M-tier hover-surface radius; the current `controlStyles` express hover surfaces through the `$ElementCornerRadius` list/button templates, so it is reserved rather than directly referenced.

### Layout & Sizing

| File | Token | Value | Notes |
|---|---|---|---|
| Taskbar | `Height` / `Width` | `42` / `42` | Standardized taskbar button/icon footprint (referenced as `$Height`/`$Width`) |
| Notification Center | `thumbnailImageSize` | `70` | Toast/thumbnail sizing |

## Base Mods

### 1. `windows-11-taskbar-styler.yml`

- **Windhawk Mod ID:** `windows-11-taskbar-styler`
- **Target Process:** `explorer.exe`
- **Framework:** WinUI 3 / XAML
- **Goal:** Glass-rounded taskbar with consistent button states, centered/aligned icon treatments, and styled Start/search elements.

**Key styled surfaces:**

- **Taskbar glass panel** (`Taskbar.TaskbarBackground#HoverFlyoutBackgroundControl`): `Background=$Background`, rim (`BorderBrush=$BorderBrush`), `BorderThickness=0.3,1,0.3,1`, `CornerRadius=$R1` (`6`). The taskbar is **no longer a detached floating dock** — there is no margin and no large frame radius.
- **Native background fill/stroke** (`Taskbar.TaskbarFrame > Grid#RootGrid > Taskbar.TaskbarBackground > Grid > Rectangle#BackgroundFill`, `Rectangle#BackgroundStroke`): both **collapsed** (`Visibility=1`) so the glass panel is the only visible taskbar background.
- **Task list button pills** (`Taskbar.TaskListButtonPanel@CommonStates > ... Border#BackgroundElement`): Per-state backgrounds (`ActiveNormal/PointerOver/Pressed`, `Inactive*`, `MultiWindow*`) using `$ActiveColor`/`$OverlayColor`/`$OverlayColor2`/`$Background`, `CornerRadius=$R1`, `Padding=6`, `Margin=2` for pill-like hit areas.
- **Search icon button** (`SearchUx.SearchUI.SearchIconButton > ... SearchButtonRootGrid#SearchBoxButtonRootPanel`): Glass background+rim, `CornerRadius=$R1`, `Height=$Height` (`42`); its nested `Border#BackgroundElement` is collapsed.
- **Icon panels & labeled button panels** (`Grid#IconPanel@CommonStates > Border#BackgroundElement`, `Taskbar.TaskListLabeledButtonPanel@CommonStates > Border#BackgroundElement`): Standardized `$Height`/`$Width` (`42x42`), `CornerRadius=$R1`, same state mapping as task list buttons.
- **Start button root panel** (`Taskbar.TaskListButtonPanel#ExperienceToggleButtonRootPanel`): Neutralized `TranslateTransform` (`0,0`).
- **Running indicator** (`Grid#IconPanel@RunningIndicatorStates > Rectangle#RunningIndicator`, `Taskbar.TaskListLabeledButtonPanel@RunningIndicatorStates > Rectangle#RunningIndicator`): Thin rounded bar (`RadiusX/Y=1.5`, `Height=4`, `Width=12`), widening to `21` when `@ActiveRunningIndicator`; filled with `$ActiveColor` (`$AccentColor` when active).
- **Overflow flyout** (`Grid#OverflowRootGrid > Border`): Transparent background/border with `BorderThickness=0` (no glass), `Padding=6`, shadow removed (`Shadow=`).

**Notes:** Heavy use of `@CommonStates` and `@RunningIndicatorStates` with per-state `Background`/`Fill`/`Width` to preserve native state semantics while applying glass materials.

### 2. `windows-11-start-menu-styler.yml`

- **Windhawk Mod ID:** `windows-11-start-menu-styler`
- **Target Process:** `StartMenuExperienceHost.exe` (also targets LockApp surfaces: `StackPanel#TimeAndDatePanel`, `TimePanel#Time`, `Date`, `WidgetFrameGrid`, `MediaTransportControls/Container`)
- **Framework:** UWP / WinUI 2
- **Goal:** Command Center glass treatment for Start Menu frame, top search pill, and Lock Screen clock/date/widgets/media controls with intentional layout tweaks.

**Key styled surfaces:**

- **Lock Screen clock/date**: Large centered time with transform group (translate/scale) and `Morganite SemiBold`; date positioned below with `vivo Sans EN VF`, both using the `ClockBG` accent brush.
- **Lock Screen widgets frame** (`Grid#WidgetFrameGrid`): Glass background+rim, `CornerRadius=$CornerRadius` (`35`).
- **Widget canvas** (`Grid#WidgetCanvasPanel`): Centered with vertical translate (`Y=50`) for positioning.
- **Lock Screen media transport/controls** (`Grid#MediaTransportControls`, `Grid#MediaControlsContainer`): Glass treatment, `CornerRadius=$CornerRadius` (`35`); container visibility collapsed and positioned (`RenderTransform` translate `Y=-250`, margin 0).
- **Start Menu foundation**: `StartDocked.StartSizingFrame` width `750`; `Grid#MainMenu` sized to `470x740`.
- **Main menu glass panel**: `Grid#Root > Border` is the glass panel (`Background=$Background`, rim, `CornerRadius=$CornerRadius` (`35`)). `Border#DropShadowDismissTarget` is **collapsed** (`Visibility=1`) and is **not** the glass panel.
- **Collapsed native chrome**: `Grid#MainMenu > Border#AcrylicBorder` is collapsed, while `Grid#CompanionRoot > Border#AcrylicBorder` is styled as glass (a more specific `StartDocked.StartMenuCompanion#RightCompanion > Grid#CompanionRoot > Border#AcrylicBorder` rule additionally sets its `Visibility=1`); `Border#AcrylicOverlay`, the drop shadows (`RootGridDropShadow`, `StartDropShadow`, `RightCompanionDropShadow`, `dropshadow`), and the accent/layer borders (`AccentLayerBorder`, `LayerBorder`) are collapsed (`Visibility=1`).
- **Top search pill** (`StartMenu.SearchBoxToggleButton#SearchBoxToggleButton`): Sized `350x50`; `Grid > Border#BorderElement` gets glass background+rim with `CornerRadius=$SearchBoxRadius` (`25`). Placeholder text updated to `"Search This Precision"`.
- **Taskbar search box (StartDocked)** (`Cortana.UI.Views.CortanaRichSearchBox#SearchTextBox > Grid > Border#BorderElement`): Glass border+background with radius `6`.
- **Flyouts, menus & overflow**: `FlyoutPresenter`/`MenuFlyoutPresenter` backgrounds, `MenuFlyoutItem`/`ToggleMenuFlyoutItem`, and `Border#OverflowFlyoutBackgroundBorder` use `CornerRadius=$FlyoutCornerRadius` (`6`).
- **Tooltips** (`ToolTip > ContentPresenter#LayoutRoot`): Glass background+rim with `CornerRadius=$ElementCornerRadius` (`10`).
- **Hover/list surfaces** (`ListViewItem` / `Button` / `ContentBorder` `#BackgroundBorder` / `#BorderBackground` templates): rim border with `CornerRadius=$ElementCornerRadius` (`10`).

**Notes:** Mixes layout transforms/positioning (translate/scale) for Lock Screen visual composition and collapses multiple native shadows/overlays/acrylic borders to favor the unified glass layer. `HoverCornerRadius` (`15`) is declared in `styleConstants` as the M-tier hover radius; current hover surfaces resolve through `$ElementCornerRadius`, so it is reserved rather than directly referenced.

### 3. `windows-11-notification-center-styler.yml`

- **Windhawk Mod ID:** `windows-11-notification-center-styler`
- **Target Process:** `ShellExperienceHost.exe` / `ShellHost.exe`
- **Framework:** UWP `Windows.UI.Xaml`
- **Goal:** Frosted glass treatment for Notification Center grid, Calendar center, Control Center region, Quick Settings/pages, toast views, and flyouts/jump lists with layered inner cards.

**Key styled surfaces:**

- **Notification Center & Calendar** (`Grid#NotificationCenterGrid`, `Grid#CalendarCenterGrid`): Glass background+rim, `PanelRadius=13`, shadows removed (`Shadow=`), calendar has margin and min height.
- **Calendar inner surfaces** (`ScrollViewer#CalendarControlScrollViewer`, `Border#CalendarHeaderMinimizedOverlay`): `ElementBackground` with `CardRadius=10`, negative margins to align layering, height on header overlay.
- **Focus Session** (`ActionCenter.FocusSessionControl#FocusSessionControl > Grid#FocusGrid`): Layered card with `ElementBackground`, `CardRadius=10`, margins.
- **Flyouts & Jump Lists** (`MenuFlyoutPresenter`, `Border#JumpListRestyledAcrylic`): Glass treatment with `ChipRadius=6`, tight padding/margins, shadows removed.
- **Control Center region** (`Grid#ControlCenterRegion`): Main glass panel, `PanelRadius=13`, bottom margin adjusted.
- **Quick Settings pages** (`ContentPresenter#PageContent`, `ContentPresenter#PageContent > Grid > Border`): Page content made transparent, inner borders/cards get `ElementBackground` with `CardRadius=10` and margins.
- **Full-screen page window** (`QuickActions.ControlCenter.AccessibleWindow#PageWindow > ContentPresenter > Grid#FullScreenPageRoot`): Transparent background, shadows removed.
- **Page header** (`FullScreenPageRoot > ContentPresenter#PageHeader`): Layered `ElementBackground` card, `CardRadius=10`, margins.
- **List content** (`ScrollViewer#ListContent`): `ElementBackground` card, `CardRadius=10`, horizontal margins.
- **Toggle/Split buttons** (`QuickActions.AccessibleToggleButton#ToggleButton`, `ControlCenter.PaginatedToggleButton#ToggleButton`, `#SplitL2Button`, `Button#SplitL2Button`): `CornerRadius=$CardRadius` with state backgrounds (`ElementBackground` → `OverlayColor2` → `OverlayColor`, checked → `AccentColor`).
- **Media transport** (`Grid#MediaTransportControlsRegion`, `Grid#ThumbnailImage`): Glass panel with `PanelRadius=13`; thumbnail sized to `$thumbnailImageSize` (`70`) with `CardRadius=10`; previous/play-pause/next buttons use `OverlayColor2`/`AccentColor`/`OverlayColor` with `ChipRadius=6`.
- **Toasts** (`ActionCenter.FlexibleToastView#FlexibleNormalToastView`): Background set to transparent, while the glass is applied to `Border#ToastBackgroundBorder` / `Border#ToastBackgroundBorder2` (`$Background`, rim, `CardRadius=10`).

**Notes:** Emphasizes inner `ElementBackground` cards to create frosted layering inside the main glass panels; removes shadows and neutralizes some page backgrounds to let the unified glass show through.

### 4. `windows-11-file-explorer-styler.yml` (deferred)

- **Windhawk Mod ID:** `windows-11-file-explorer-styler`
- **Target Process:** `explorer.exe`
- **Framework:** WinUI 3 `Microsoft.UI.Xaml`
- **Status:** ⏸️ Deferred (Milestone M.03) — **no styler file exists** in this directory.
- **Rationale (user directive, 2026-10-07):** Deferred due to a lack of plausible customizations — other already-existing styles are too similar, so there is no real benefit to having our own File Explorer style just yet. The styler file was generated, then discontinued (git `1cc49e9`).

**Dormant reference:** Should the surface be resumed, intended coverage is WinUI 3 tabs, navigation, command bar, address bar, file list chrome, panes, and flyouts, respecting the Win32 file list boundary and the Safe Glass Chrome principle.

**Notes:** Any future attempt must avoid intrusive third-party translucency injectors (e.g. TranslucentWindows) that affect other programs. Do not scaffold a File Explorer styler file without a new explicit user directive.

## Style Constants Reference

All three active files define the same core constants (blur materials, `Background`, `BorderBrush`, `BorderThickness`) plus the additive tokens listed below. Use `$Token` references in `styles` for consistency.

| Constant | Type | File(s) | Purpose |
|---|---|---|---|
| `Translucent` | `WindhawkBlur` | All | Light blur accent |
| `Glass` | `WindhawkBlur` | All | Ultra-light glass |
| `Frosted` | `WindhawkBlur` | All | Base panel material |
| `Acrylic` | `WindhawkBlur` | All | Strong blur |
| `Background` | alias `$Frosted` | All | Canonical background |
| `BorderBrush` | `LinearGradientBrush` (vertical rim) | All | Top-lit glass rim |
| `BorderThickness` | thickness (`0.3,1,0.3,1`) | All | Rim thickness |
| `BorderBrush2` | `WindhawkBlur` (alt) | Start, NC | Soft alternate border |
| `OverlayColor` | `AcrylicBrush` | Taskbar, NC | Active/pointer-over fill |
| `OverlayColor2` | `AcrylicBrush` (lower luminosity) | Taskbar, NC | Inactive/multi-window/alt hover |
| `AccentColor` | `AcrylicBrush` (system accent) | Taskbar, NC | Accent highlights |
| `ActiveColor` | `AcrylicBrush` (opaque) | Taskbar | Active pill/indicator fill |
| `ElementBackground` | `SolidColorBrush` (low opacity) | NC | Layered inner cards |
| `ClockBG` | `SolidColorBrush` (accent, solid) | Start | Lock screen clock/date |
| `CornerRadius` / `SearchBoxRadius` / `ElementCornerRadius` / `FlyoutCornerRadius` / `HoverCornerRadius` | radii | Start | Harmonized radii scale |
| `R1` / `R2` | radii | Taskbar | Small/large taskbar radii |
| `PanelRadius` / `CardRadius` / `ChipRadius` | radii | NC | Harmonized radii scale |
| `Height` / `Width` | size (`42` / `42`) | Taskbar | Taskbar button footprint |
| `thumbnailImageSize` | size (`70`) | NC | Thumbnail sizing |

## Installation & Usage

1. **Install Windhawk**: [windhawk.net](https://windhawk.net/)
2. **Install the official Windhawk Styler mod(s)** for each surface you want styled:
   - Taskbar: `windows-11-taskbar-styler`
   - Start Menu: `windows-11-start-menu-styler`
   - Notification Center: `windows-11-notification-center-styler`
   - *(File Explorer: `windows-11-file-explorer-styler` is approved but deferred — no suite config exists yet.)*
3. **Apply the YAML**: In each mod's settings, import/copy the corresponding YAML from this directory (`projects/command-center/`). Each YAML is self-contained with `styleConstants` and `controlStyles`.
4. **Theme awareness**: Styles read Windows theme resources and should adapt to light/dark automatically.

> **Process targets:** Taskbar → `explorer.exe`. Start Menu → `StartMenuExperienceHost.exe`. Notification Center → `ShellExperienceHost.exe`/`ShellHost.exe`. (File Explorer would target `explorer.exe` if resumed.)

## Validation & Quality Gates

This repo enforces static validation and live verification.

- **Static gate (mandatory):** Run syntax/token validation with the PowerShell linter:

  ```powershell
  pwsh -NoProfile -File tools/Test-WindhawkStyles.ps1
  ```

  The suite must pass with `0 errors` before considering changes complete.

- **Live verification:** Validate on desktop against a manual checklist covering each active surface (Taskbar, Start Menu/Lock, Notification Center/Quick Settings), states (normal/hover/active), theme switching, and regression spots. File Explorer remains deferred and is not covered.

- **Target evidence (Rule 04):** Selectors must be sourced from official Windhawk mod source code, settings schemas, or community theme references. Avoid guessing; prefer evidence-backed targets.

## Conventions & Compliance

All changes must comply with the repository standards below:

- **Rule 01 – Zero unsolicited injection:** Four approved styler mods (three active files; File Explorer deferred); no unapproved packages/tools.
- **Rule 02 – Windhawk Styler syntax:** YAML/XAML syntax, quoting, constant declaration order, no inline syntax errors.
- **Rule 03 – Design language standards:** Use canonical "Command Center Glass" tokens/materials and radius scales above.
- **Rule 04 – Target evidence protocol:** Sourced selectors only; document evidence when adding/changing targets.
- **Rule 05 – Surface scope standards:** Respect process targets and WinUI 3 vs UWP boundaries.
- **Rule 07 – Verification standards:** Mandatory static gate + user live checklist.
- **Rule 08 – Documentation standards:** Root Markdown files + per-project `README.md` files are the canonical indexes; `docs/` hosts the GitHub Pages documentation site.
- **Rule 09 – Milestone & sprint tracking:** Work is tracked via milestones/sprints and date/time-grouped `CHANGELOG.md` entries — the project does not use formal versioned releases. (Roadmap, sprint, and task ledgers live in the workspace agents ecosystem.)

## Related Documentation

- **Repository root:** [`README.md`](../README.md)
- **Extras & assets:** [`extras/README.md`](extras/README.md)
- **Validation tool:** [`tools/Test-WindhawkStyles.ps1`](../tools/Test-WindhawkStyles.ps1)
- **Changelog:** [`CHANGELOG.md`](../CHANGELOG.md) — chronological log of completed work (entries grouped by date and time; tracked via milestones/sprints, not versioned releases)
# Radar Beam Width Calculator — Improvements & Requirements

This document defines all planned improvements for the Radar Beam Width Calculator. Phase 1–3 are the active work items with detailed requirements. The Backlog section tracks future ideas.

**Tech stack:** Plain HTML, CSS, JavaScript — no frameworks, no build tools. Static site on GitHub Pages.

**Brand:** Banner yellow (#FFC107) as primary accent. Clean, professional, mobile-first.

---

## ~~Phase 1: Visual Polish~~ ✅ Completed 2026-02-14

All items in this phase have been implemented.

### ✅ 1.1 Unit System Toggle → Segmented Control
### ✅ 1.2 Visual Hierarchy — Section Headers & Input Labels
### ✅ 1.3 Remove Helper Text Under Inputs
### ✅ 1.4 Tighten Input Fields
### ✅ 1.5 Results Card — Increased Contrast
### ✅ 1.6 Better Empty State

---

## Phase 2: Reverse Calculation Mode

Adds a new calculation direction: given a target spot size, find the maximum mounting distance.

### 2.1 Mode Toggle

**Requirements:**
* Add a second segmented control below the unit system toggle
* Two options: Beam Width | Max Distance
  * "Beam Width" = forward calculation (default, current behavior)
  * "Max Distance" = reverse calculation
* Style as a secondary/outlined variant to visually distinguish from the unit toggle:
  * Option A: Banner yellow border + yellow text on active segment, no filled background
  * Option B: Different visual treatment (thinner, lighter) so the two toggles are clearly distinct
* Optional small label above: "Calculate" (in section header style)

### 2.2 Reverse Calculation — Inputs

When "Max Distance" is active, the form reconfigures:

1. **Sensor Model** — Same dropdown as forward mode (including Custom with manual beam angle entry)
2. **Target Vertical Spot Size** — Numeric input
   * Label: "Target Vertical Spot Size"
   * Metric mode: input in meters
   * Imperial mode: Feet + Inches (same dual-field pattern as forward distance input)
3. **Target Horizontal Spot Size** — Numeric input
   * Label: "Target Horizontal Spot Size"
   * Same unit behavior as above

### 2.3 Reverse Calculation — Output

Results card displays:
* "Max distance for [V] vertical spot: [Y] m" (or ft/in in Imperial)
* "Max distance for [H] horizontal spot: [Y] m" (or ft/in in Imperial)
* Practical limit line: "Usable mounting distance: [shorter of the two]" — this is the real-world answer since the sensor must satisfy both constraints simultaneously

When a preset sensor is selected, show the sensor model name in the results card (same as forward mode per 1.5).

### 2.4 Reverse Calculation — Math

**Forward formula:**
```
beam_width = 2 × distance × tan(beam_angle_in_degrees × π / 360)
```

**Reverse formula:**
```
distance = target_spot_size / (2 × tan(beam_angle_in_degrees × π / 360))
```

### 2.5 Mode Toggle Behavior

* Switching modes preserves unit system selection (Imperial/Metric)
* Switching modes preserves selected sensor preset
* Form transition should use CSS transitions to smoothly show/hide appropriate fields
* Results update in real-time as user types (same as current forward mode)

---

## Phase 3: Compare Mode

Shows beam width (or max distance) for all three core sensors simultaneously.

### 3.1 Activation

* Add a "Compare All Sensors" toggle or button below the Sensor Model dropdown
* When activated, the Sensor Model dropdown is disabled or hidden
* Style as a secondary action: outlined button with Banner yellow border, or a checkbox/toggle
* Compare mode works in both Beam Width and Max Distance calculation modes

### 3.2 Compare — Forward Calculation ("Beam Width" mode)

**Input:** Distance only (same field as standard mode)

**Output:** Three results cards, stacked vertically on mobile (12px gap between cards):

| Sensor | Vertical Angle | Horizontal Angle |
|--------|----------------|------------------|
| T30R   | 15°            | 15°              |
| K50R   | 40°            | 30°              |
| Q90R   | 40°            | 40°              |

Each card displays:
* Sensor model name as card header (bold, 16px)
* Beam angles in muted text below name (e.g., "15° × 15°")
* Vertical Beam Width result
* Horizontal Beam Width result

Use the same card styling from Phase 1.5 (white/tinted background, shadow, yellow left border).

### 3.3 Compare — Reverse Calculation ("Max Distance" mode)

**Inputs:** Target Vertical Spot Size + Target Horizontal Spot Size (same inputs as standard reverse mode, no sensor selection needed)

**Output:** Three results cards (same layout), each showing:
* Max distance based on vertical spot constraint
* Max distance based on horizontal spot constraint
* Practical mounting distance (shorter of the two)

### 3.4 Compare Behavior

* All three sensors always display when compare is active (no selection UI)
* Results update in real-time as inputs change
* Exiting compare mode restores the previously selected sensor preset
* Toggling compare on/off preserves all other form state (units, calc mode, input values)

---

## Implementation Notes

### Sensor Data Structure

Define sensor data as a top-level JavaScript object for easy maintenance. New sensors should be addable without modifying calculation or UI logic:

```javascript
const SENSORS = [
  { id: 't30r', name: 'T30R', vAngle: 15, hAngle: 15 },
  { id: 'k50r', name: 'K50R', vAngle: 40, hAngle: 30 },
  { id: 'q90r', name: 'Q90R', vAngle: 40, hAngle: 40 },
];
```

**Note:** The Q90R2 also exists in the current preset dropdown — keep it in the dropdown for individual use, but it is not included in Compare mode.

### CSS Variables

Use custom properties for consistent branding:

```css
:root {
  --banner-yellow: #FFC107;
  --banner-yellow-light: #FFF8E1;
  --text-primary: #1a1a1a;
  --text-secondary: #666;
  --border-color: #ddd;
  --card-shadow: 0 2px 8px rgba(0,0,0,0.1);
}
```

### Accessibility

* All interactive elements must be keyboard-navigable
* Maintain WCAG AA color contrast minimums
* Segmented controls should use role="tablist" / role="tab" or radio button semantics
* Results should use aria-live="polite" so screen readers announce updates

### Performance

* No external dependencies or CDN calls — tool must work offline after initial load
* All calculations are simple trigonometry — no performance concerns

---

## Backlog (Future Ideas)

These are not part of the current work. Tracked here for future consideration.

### Remember Unit Preference

**Effort:** Medium · **Impact:** Medium

Use localStorage to persist Imperial/Metric preference between sessions. Graceful degradation if unavailable.

### Copy / Share Results

**Effort:** Medium · **Impact:** Medium

Add a "Copy" button to copy formatted calculation results to clipboard. Include all parameters (distance, sensor, results). Show "Copied!" confirmation.

### Input Validation Indicators

**Effort:** Medium · **Impact:** Medium

Subtle positive visual feedback (green border or checkmark) on valid inputs, not just error states. Don't overdo it. Don't rely solely on color for accessibility.

### Visual Beam Diagram

**Effort:** High · **Impact:** High

Top-down SVG or Canvas diagram showing beam coverage area as a triangle/cone scaled to calculated dimensions. Complex but high-value for visualization.

### URL State Persistence

**Effort:** Medium · **Impact:** Low

Encode calculation parameters in URL query string so users can bookmark or share specific calculations. Update on input change (debounced). Load from URL on page load with fallback to defaults.

### Keyboard Navigation Enhancement

**Effort:** Low · **Impact:** Low

Add Enter key support or visual feedback. May not be necessary given auto-calculation behavior.

---

## Completed

### ✅ Phase 1: Visual Polish
**Completed:** 2026-02-14

Segmented control for unit toggle, improved visual hierarchy (section headers vs. input labels), removed helper text, tightened input field widths, restyled results card with shadow and sensor name display, improved empty state messaging.

### ✅ Collapsible Custom Angle Inputs
**Completed:** 2025-01-24

Custom beam angle inputs only appear when "Custom" is selected from the preset dropdown.

### ✅ Add Sensor Names to Presets
**Completed:** 2025-01-24

Added sensor model names (T30R, K50R, Q90R, Q90R2) to preset dropdown options.

# Claude Context: Radar Beam Width Calculator

This document provides context about the Radar Beam Width Calculator project for AI assistants working on this codebase.

## Project Overview

**Purpose:** A single-page web calculator that helps users determine the beam width coverage area of Banner Engineering radar sensors at various distances.

**Target Users:** Field engineers, installation technicians, and sales engineers who need to quickly calculate sensor coverage dimensions for installation planning.

**Key Use Case:** User enters a distance from sensor to target area, selects a sensor model (or enters custom beam angles), and receives the vertical and horizontal beam width dimensions at that distance.

## Architecture

**Technology Stack:**
- Pure vanilla JavaScript (no frameworks)
- Single HTML file (`index.html`) - fully self-contained
- No build process or dependencies
- No external CSS or JS files

**Why Single File:**
- Maximum portability and ease of deployment
- Can be hosted anywhere or run locally
- No dependency management needed
- Fast loading and simple maintenance

## File Structure

```
radar-calc/
├── index.html          # Complete application (HTML, CSS, JS)
├── IMPROVEMENTS.md     # Tracked feature ideas and enhancements
├── claude.md          # This file - AI assistant context
└── README.md          # User-facing documentation (if exists)
```

## Key Components

### HTML Structure (Lines ~254-331)

1. **Unit Toggle** - Imperial (feet/inches) ⟷ Metric (meters)
2. **Distance Inputs** - Either feet+inches OR meters (toggle switches between them)
3. **Beam Angles Section:**
   - Preset sensor configurations dropdown (T30R, K50R, Q90R, Q90R2)
   - Collapsible custom angle inputs (only visible when "Custom" selected)
4. **Output Section** - Displays calculated vertical and horizontal beam widths

### CSS Styling (Lines ~7-277)

**Brand Colors:** Banner Engineering color scheme
- Primary: Yellow (#FFD600)
- Text: Black and gray scale
- Clean, professional aesthetic

**Key Style Features:**
- Collapsible custom angle inputs with smooth transitions (`.custom-angles`)
- Toggle switch styling for Imperial/Metric
- Responsive design for mobile devices
- Accessibility-focused (ARIA labels, focus states, contrast)

### JavaScript Logic (Lines ~333-543)

**Core Functions:**

1. **`calculateBeamWidth()`** (Lines ~389-471)
   - Main calculation engine with 300ms debounce
   - Formula: `width = 2 × distance × tan(angle/2)`
   - Validates inputs and shows errors
   - Formats output based on unit system

2. **`convertToMeters()` / `convertToFeetInches()`** (Lines ~351-367)
   - Unit conversion with rounding bug fix (handles inches ≥ 12)

3. **Preset Handler** (Lines ~507-521)
   - Toggles custom input visibility
   - Populates angle values from preset

4. **`resetPresetToCustom()`** (Lines ~524-528)
   - Switches to custom mode when angles manually edited

## Important Implementation Details

### Sensor Presets
Current sensor models with beam angles (vertical × horizontal):
- **T30R**: 15° × 15°
- **K50R**: 40° × 30°
- **Q90R**: 40° × 40°
- **Q90R2**: 40° × 120°

**Note:** Angles in dropdown are ordered as **vertical × horizontal** to match sensor naming conventions.

### Collapsible Custom Inputs
- Default state: Hidden (only preset dropdown visible)
- Custom inputs use CSS transitions for smooth slide effect
- Max-height trick for smooth height animation
- De-emphasized styling (smaller font, lighter color) when visible

### Unit System Behavior
- Toggle converts values between systems automatically
- Preserves calculation state during toggle
- Handles edge case: inches rounding to 12 (converts to 1 ft 0 in)

### Input Validation
- Distance must be > 0
- Angles must be 1-180°
- Both angles required for calculation
- Empty state shows "Enter values to calculate"
- Errors display with red styling and clear messages

## Development Conventions

### Code Style
- Use vanilla JavaScript (no jQuery, no frameworks)
- Debounce calculations (300ms) for performance
- Clear variable names (e.g., `beamAngleVInput`, `outputH`)
- Comments explain "why" not "what"

### Editing Guidelines
1. **Always read file before editing** - Use Read tool first
2. **Keep it simple** - Avoid over-engineering
3. **Maintain single-file architecture** - Don't split into multiple files
4. **Test edge cases** - Especially unit conversions and validation
5. **Preserve accessibility** - Keep ARIA labels and focus states

### What NOT to Do
- Don't add external dependencies or frameworks
- Don't create separate CSS/JS files
- Don't add features "for the future" - keep it minimal
- Don't remove accessibility features
- Don't add unnecessary comments or documentation in code

## Common Tasks

### Adding a New Sensor Preset
1. Add `<option>` in HTML with format: `SensorName - V° x H°`
2. Value attribute: `"vertical,horizontal"` (comma-separated)
3. Order matters: vertical first, then horizontal

### Changing Styling
- Colors: Modify CSS variables in `:root` (lines ~9-18)
- Layout: Look for specific class names (`.custom-angles`, `.output`, etc.)
- Responsive: Check `@media` queries (lines ~260-277)

### Modifying Calculations
- Main formula in `calculateBeamWidth()` around line ~449-450
- Use radians: `(angle * Math.PI / 180)`
- Validate inputs before calculating
- Handle both unit systems in output formatting

## Testing Checklist

When making changes, manually verify:
- [ ] Both Imperial and Metric modes work
- [ ] All sensor presets populate correctly
- [ ] Custom angles appear/hide properly
- [ ] Unit toggle converts values correctly
- [ ] Edge cases: 0 distance, 180° angles, 11.99 inches
- [ ] Error messages display for invalid inputs
- [ ] Output formatting is correct in both unit systems
- [ ] Mobile/responsive layout works
- [ ] Keyboard navigation functions

## Related Documentation

- **IMPROVEMENTS.md** - Tracked future feature ideas and enhancements
- **Git Branch Convention** - Use `claude/description-xxxxx` format for branches

## Project Philosophy

**Guiding Principles:**
1. **Speed over features** - Fast, focused tool beats feature-bloated app
2. **Simplicity over flexibility** - Common use case done well beats handling every edge case
3. **Portability over optimization** - Single file beats better architecture
4. **User attention management** - Show what's needed, hide what's not

**User Workflow Priority:**
1. Enter distance (most common/important)
2. Select sensor preset (primary interaction)
3. View results (goal)
4. Custom angles (advanced/rare use case)

## Known Issues / Quirks

- None currently tracked

## Future Considerations

See `IMPROVEMENTS.md` for prioritized list of potential enhancements. Top candidates:
- Show selected sensor model in output
- Remember unit preference (localStorage)
- Visual beam diagram
- Comparison mode for all sensors

---

**Last Updated:** 2026-02-06
**Primary Maintainer:** schwirtzy
**Claude Sessions:** This file helps maintain context across multiple AI assistant sessions

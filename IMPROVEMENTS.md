# Radar Beam Width Calculator - Improvement Ideas

This document tracks potential enhancements, usability improvements, and feature ideas for future development.

---

## High Priority / Quick Wins

### 1. Show Selected Sensor in Output
**Status:** Not Started
**Effort:** Low
**Impact:** High

Display the sensor model name (T30R, K50R, etc.) in the output section when a preset is selected. This helps users verify what they calculated for and improves traceability.

**Implementation Notes:**
- Add a new output line showing "Sensor: [Model Name]" or similar
- Only display when a preset is selected (hide for custom)
- Could be placed above or below the beam width results

---

### 2. Update Preset Dropdown Label
**Status:** Not Started
**Effort:** Very Low
**Impact:** Medium

Change "Preset Configurations" label to "Sensor Model" since that's what the options actually represent.

**Implementation Notes:**
- Simple text change in HTML
- Update hint text accordingly

---

### 3. Add Degree Symbol to Custom Angle Labels
**Status:** Not Started
**Effort:** Very Low
**Impact:** Low

Add degree symbol (°) to custom angle input labels for clarity.

**Current:** "Vertical Beam Angle (degrees)"
**Proposed:** "Vertical Beam Angle (°)" or similar

---

### 4. Better Empty State Messaging
**Status:** Not Started
**Effort:** Low
**Impact:** Medium

Replace generic "Enter values to calculate" with more specific guidance like "Enter distance and select sensor model".

**Implementation Notes:**
- Could show different messages based on what's missing
- More actionable guidance for users

---

## Medium Priority

### 5. Remember User's Unit Preference
**Status:** Not Started
**Effort:** Medium
**Impact:** Medium

Use localStorage to persist the user's Imperial/Metric preference between sessions.

**Implementation Notes:**
- Save preference on toggle change
- Load on page initialization
- Graceful degradation if localStorage unavailable

---

### 6. Input Validation Indicators
**Status:** Not Started
**Effort:** Medium
**Impact:** Medium

Show positive visual feedback (green checkmarks, borders) on valid inputs, not just error states.

**Implementation Notes:**
- Add visual indicators for valid states
- Don't overdo it - keep it subtle
- Consider accessibility (don't rely solely on color)

---

### 7. Copy/Share Results
**Status:** Not Started
**Effort:** Medium
**Impact:** Medium

Add a "Copy" button to easily copy calculation results to clipboard.

**Implementation Notes:**
- Format copied text appropriately
- Could include all parameters (distance, sensor, results)
- Show "Copied!" confirmation feedback

---

### 8. Keyboard Navigation Enhancement
**Status:** Not Started
**Effort:** Low
**Impact:** Low

Add Enter key support to trigger calculation when focused in input fields.

**Implementation Notes:**
- Already auto-calculates on input
- Could add visual feedback or "Calculate" button for explicit action
- Consider if this is actually needed given auto-calculation

---

## Nice to Have / Future Features

### 9. Visual Beam Diagram
**Status:** Not Started
**Effort:** High
**Impact:** High

Show a simple top-down diagram illustrating the beam coverage area.

**Implementation Notes:**
- Could use Canvas or SVG
- Show triangle/cone representing beam spread
- Scale appropriately with calculated dimensions
- This would be complex but very valuable for visualization

---

### 10. Reverse Calculator
**Status:** Not Started
**Effort:** High
**Impact:** Medium

Allow users to enter desired beam width and calculate required distance.

**Implementation Notes:**
- Would require UI redesign or mode toggle
- Solve equation backwards: distance = width / (2 × tan(angle/2))
- Consider if this is a common use case

---

### 11. URL State Persistence
**Status:** Not Started
**Effort:** Medium
**Impact:** Low

Encode calculation parameters in URL so users can bookmark or share specific calculations.

**Implementation Notes:**
- Use URL query parameters
- Update on input change (debounced)
- Load from URL on page load
- Fallback to defaults if invalid

---

### 12. Comparison Mode
**Status:** Not Started
**Effort:** High
**Impact:** Medium

Show beam widths for all sensor models at once for a given distance.

**Implementation Notes:**
- Could be a toggle/tab to switch to comparison view
- Table or grid showing all sensors
- Helps users choose the right sensor for their needs

---

## Completed Improvements

### ✅ Collapsible Custom Angle Inputs
**Completed:** 2026-01-24

Made custom beam angle inputs collapsible and de-emphasized. They now only appear when "Custom" is selected from the preset dropdown, focusing user attention on distance input, sensor presets, and output.

### ✅ Add Sensor Names to Presets
**Completed:** 2026-01-24

Added sensor model names (T30R, K50R, Q90R, Q90R2) to preset configuration options for better clarity.

---

## Notes for Future Development

- Maintain focus on simplicity and speed of use
- Avoid over-engineering - prioritize features users actually need
- Test any changes with actual end users if possible
- Consider mobile/touch experience for any new features
- Keep accessibility in mind (keyboard navigation, screen readers, color contrast)

---
name: bdd-implement
description: BDD-first implementation skill for UI changes and bug fixes. Validates from end-user/QA perspective using XCUITest or Playwright — inspect current state, screenshot before, fix, screenshot after, confirm visually. Use for any visible UI change or bug on macOS or web.
---

# BDD Implement

## When to Use

Invoke this skill for:
- Any bug in a macOS app UI
- Any bug in a web app UI
- Any feature that changes visible UI behavior
- Any time a fix could be "correct" per code but wrong visually

**Anti-pattern this replaces:** Writing unit tests that verify code behavior when the actual requirement is a visual/UX outcome. Unit tests do not validate "the button looks right" or "the modal closes correctly."

## Workflow

### Phase 1: Inspect Current State

**macOS app:**
Use XCUITest to inspect the accessibility tree and capture the current state of UI elements before any changes.

**Web app:**
Use `playwright_browser_snapshot` to get current accessibility state.
Use `playwright_browser_take_screenshot` to capture current visual state.

Document what you find: element labels, positions, states.

### Phase 2: Screenshot Before

Capture the **failing or pre-change state** visually.

**macOS:** Use XCUITest screenshot API: `XCUIScreen.main.screenshot()` or `app.screenshot()`.
**Web:** `playwright_browser_take_screenshot` with `filename: "before-fix.png"`.

Label it clearly: `before-<issue-description>.png`.

### Phase 3: Implement

Make the **minimal change** required. Do not over-engineer. Follow YAGNI.

### Phase 4: Screenshot After

Capture the **fixed/changed state** using the same method as Phase 2.

Label it: `after-<issue-description>.png`.

### Phase 5: Compare and Validate

Side-by-side comparison:
- Does the visual output match the requirement?
- Did anything else change unexpectedly?
- Does the accessibility tree reflect the correct semantic state?

If something unexpected changed: stop, diagnose, fix, re-screenshot.

### Phase 6: Advisor Completion Gate

Invoke `advisor-gate` completion gate with:
- Before screenshot path
- After screenshot path
- What was changed
- What requirement is being met

**Do not declare done without this gate.**

## Tools

| Context | Inspect tool | Screenshot tool |
|---------|-------------|-----------------|
| Web app | `playwright_browser_snapshot` | `playwright_browser_take_screenshot` |
| macOS web view | `playwright_browser_snapshot` | `playwright_browser_take_screenshot` |
| macOS native | XCUITest accessibility snapshot | XCUITest `screenshot()` API |

## Example Invocation

```
bdd-implement triggered for: "Button color is wrong in dark mode"

Phase 1: Inspect — accessibility snapshot shows button role=button, label="Submit"
Phase 2: Screenshot before → before-dark-mode-button.png (button shows #333 on #222 — low contrast)
Phase 3: Fix → update ColorScheme.swift buttonBackground dark variant to #0A84FF
Phase 4: Screenshot after → after-dark-mode-button.png (button shows blue on dark — correct)
Phase 5: Compare → color fixed, no other elements changed
Phase 6: Completion gate → advisor APPROVE
```

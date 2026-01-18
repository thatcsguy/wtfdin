# Plan: UI Simplification

## 1. Remove Pre-configured BIS Lists
- Delete `JOB_BIS_PRESETS` object (lines 2440-2768)
- Delete `DEFAULT_BIS` constant (line 2756)
- Delete `getBiSPreset()` function (lines 2771-2773)
- Modify `showBisConfigStep()` - remove preset option, show only manual config
- Remove "Use Preset BiS" button path (lines 3659-3664)
- Initialize `manualSlotConfig` with all slots set to "raid" by default
- Simplify `addJobWithConfig()` - no longer compare against preset

## 2. Remove Left Navigation Bar
- Delete `<aside class="sidebar">` HTML block (lines 2081-2118)
- Delete sidebar CSS (lines 63-153, character-list styles 189-205)
- Delete `renderCharacterList()` function (lines 3767-3838)
- Remove event listener setup for sidebar elements

## 3. Redesign Top Nav
- Move title "WTFDIN" to top-left of existing top nav
- Add export button next to title
- Replace big character name with dropdown (`<select>` or custom)
- Dropdown options: all characters + "New Character" entry
- Add edit (pencil) button next to dropdown for renaming
- On "New Character" select → open add character modal
- On edit click → open rename modal for current character

## 4. Remove Progress Tracking
- Delete `calculateJobProgress()` function (lines 2786-2808)
- Remove progress bar HTML from `renderMainPanel()` (lines 4122-4125)
- Remove progress calculation code (lines 4014-4022)
- Delete `.progress-bar` CSS (lines 1578-1586)
- Optionally remove `slot-complete`/`slot-partial` classes and CSS if desired

### 5. Character Controls in Top Nav
- Edit button (pencil icon) → opens rename modal
- Delete button (trash icon) → opens delete confirmation modal
- If no characters exist → auto-open "New Character" modal

### 6. Custom Character Dropdown
- Build custom dropdown component matching app's look/feel
- Shows current character name as trigger
- Dropdown list: all characters + "New Character" divider option
- Click outside to close, keyboard accessible

# Feature Implementation Plan

## 1. Wider Job Modal
- Change `.modal` max-width from 600px → 800px
- Melee DPS (6 jobs) will fit in one row at 80px per item

## 2. Gear Selection on Job Add
- After job selection in modal, show second step: gear config
- Two options: "Use Preset BiS" button OR manual slot-by-slot selection
- For manual: grid of 11 slots, each with raid/tome toggle
- Store custom config in job's `bisOverride` field (null = use preset)

## 3. Change Gear Type on Job Page
- Add clickable toggle on each row's "Type" column
- Clicking "Raid" → "Tome" or vice versa
- Update `bisOverride` for that job when changed
- Re-render row with correct checkboxes (have vs base/upgraded)
- Validate ring rules on type change

## 4. Replace Browser Prompts with Modals
Locations using `prompt()` or `confirm()`:
- Character rename → custom input modal
- Character delete → custom confirm modal
- Job remove (×) → custom confirm modal
Create reusable modal functions: `showInputModal()`, `showConfirmModal()`

## 5. Raid Piece Turn Source
Add constant mapping:
```js
const RAID_TURNS = {
  weapon: { turn: 4, label: '4th Turn' },
  head: { turn: 2, label: '2nd Turn' },
  hands: { turn: 2, label: '2nd Turn' },
  feet: { turn: 2, label: '2nd Turn' },
  body: { turn: 3, label: '3rd Turn' },
  legs: { turn: 3, label: '3rd Turn' },
  earrings: { turn: 1, label: '1st Turn' },
  necklace: { turn: 1, label: '1st Turn' },
  bracelets: { turn: 1, label: '1st Turn' },
  ring1: { turn: 1, label: '1st Turn' },
  ring2: { turn: 1, label: '1st Turn' }
}
```
Display turn label in Type column for raid slots

## 6. Tome Piece Costs
Add constant:
```js
const TOME_COSTS = {
  weapon: 500,
  head: 495,
  body: 825,
  hands: 495,
  legs: 825,
  feet: 495,
  earrings: 375,
  necklace: 375,
  bracelets: 375,
  ring1: 375,
  ring2: 375
}
```
Display cost in Type column for tome slots

## 7. Right Side Summary Panel
Add new column/panel to right of gear table showing remaining needs:
- Group unchecked raid pieces by turn: "1st Turn: 3 drops", "3rd Turn: 1 drop"
- Sum unchecked tome pieces: "Tomestones: 2,070"
- Update dynamically as checkboxes change

---

## Implementation Order
1. Wider modal (trivial CSS)
2. Reusable modal system (needed for #4, #2)
3. Replace browser prompts
4. Raid turns + tome costs display
5. Gear type toggle on job page
6. Gear selection on job add
7. Right side summary

---

## Unresolved Questions
1. For tome weapon: show "500 + tokens" or just "500"? (tokens also come from 4th turn)
2. Summary panel: separate section to right, or additional column in table?
3. When changing slot type mid-progress, reset that slot's checkboxes or preserve state?

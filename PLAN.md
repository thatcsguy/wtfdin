# WTFDIN - What The Fuck Do I Need?

## Overview
Static HTML + vanilla JS app for FFXIV savage tier gear tracking. Local storage persistence, URL export for sharing.

## Decisions
- **Presets**: Hardcoded per job (e.g. "Dragoon BiS"). Not user-editable.
- **Rings**: No duplicate items. Valid combos: raid+tome, upgraded tome+base tome, raid+upgraded, raid+base. Invalid: 2x raid, 2x same tome state.
- **Slot names**: Generic (Weapon, Head, Body, etc.)
- **Job icons**: Real FFXIV icons (XIVAPI or similar CDN)
- **Theme**: Dark mode
- **Tome dependency**: Checking "Upgraded" auto-checks "Base"

## Data Model

### Gear Slots
- Weapon, Head, Body, Hands, Legs, Feet, Earrings, Necklace, Bracelets, Ring1, Ring2

### Item Source Types
- `raid` - dropped from savage raids
- `tome` - purchased with tomestones (upgradeable)

### Storage Structure
```
{
  characters: [
    {
      id, name,
      jobs: [
        {
          jobId,
          slots: {
            [slot]: {
              type: "raid"|"tome",
              hasRaid: bool,
              hasTomeBase: bool,
              hasTomeUpgrade: bool
            }
          }
        }
      ]
    }
  ]
}
```

### Hardcoded BiS Presets
```
const BIS_PRESETS = {
  DRG: { weapon: "raid", head: "tome", body: "raid", ... },
  WAR: { ... },
  // etc for all jobs
}
```

## File Structure
```
index.html
styles.css
app.js
```

## UI Layout

```
+------------------------------------------+
|  WTFDIN                    [Export URL]  |
+------------------------------------------+
|  Characters    |  Character: Name        |
|  -----------   |  [Delete] [Rename]      |
|  > Char 1      |                         |
|    Char 2      |  Jobs: [DRG] [WAR] [+]  |
|  [+ Add]       |                         |
|                |  +-------------------+  |
|                |  | Slot  | Type |Has |  |
|                |  |-------|------|----|  |
|                |  | Weapon| Raid | ☑  |  |
|                |  | Head  | Tome |☑|☐ |  |
|                |  | ...   |      |    |  |
|                |  +-------------------+  |
+------------------------------------------+
```

## Implementation Phases

### Phase 1: HTML + CSS Foundation
- Semantic HTML structure
- CSS custom properties for dark theme
- Flexbox layout (sidebar + main)
- Mobile responsive

### Phase 2: Job Data + Icons
- Job list with IDs, names
- XIVAPI icon URLs (or local fallback)
- BiS preset data per job

### Phase 3: State Management
- localStorage helpers (get/set/init)
- URL encode/decode (base64 JSON in query param)
- Import prompt on load if URL has data

### Phase 4: Character Management
- Render character list
- Add character (prompt name)
- Delete character (confirm)
- Rename character
- Select character → update main view

### Phase 5: Job Management
- Job picker modal (grid of job icons)
- Add job to character
- Remove job from character
- Job tabs/pills, click to select

### Phase 6: Gear Table
- Render slots from BiS preset for selected job
- Raid row: single "Have" checkbox
- Tome row: "Base" + "Upgraded" checkboxes
- Upgraded auto-checks Base
- Ring validation (no duplicates)

### Phase 7: Progress Display
- Per-job completion (X/11 slots done)
- Visual row states: incomplete, partial (tome base only), complete
- Maybe summary on character card

### Phase 8: Export/Import
- Serialize all characters to JSON
- Base64 encode → query param `?data=...`
- Copy URL button
- On load: detect `?data=`, decode, prompt "Import this data?"

### Phase 9: Polish
- Hover states, transitions
- Checkmark icons (CSS or unicode)
- Loading states
- Error handling (corrupt data, etc.)

## Job Icons Source
XIVAPI provides icons at:
`https://xivapi.com/cj/1/{jobAbbr}.png`

Fallback: Unicode symbols or text abbreviations.

## Ring Validation Logic
```js
function isValidRingCombo(ring1, ring2) {
  // Can't have two raid rings
  if (ring1.type === "raid" && ring2.type === "raid") return false;

  // Can't have two tome rings at same upgrade level
  if (ring1.type === "tome" && ring2.type === "tome") {
    const r1State = ring1.hasTomeUpgrade ? "upgraded" : "base";
    const r2State = ring2.hasTomeUpgrade ? "upgraded" : "base";
    if (r1State === r2State) return false;
  }

  return true;
}
```

## No Remaining Questions

Ready to implement.

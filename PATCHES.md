# Local patches

## Bagnon border integration

**Recorded:** 2026-11-10  
**Applied to:** ShardSource 2.0.4  
**File:** `ShardSource.lua`, function `RefreshBagnon`

### Problem

ShardSource originally looked up Bagnon with AceAddon:

```lua
local bagnon = LibStub("AceAddon-3.0"):GetAddon("Bagnon", true)
```

The installed Bagnon/BagBrother build uses WildAddon and exposes the Bagnon
instance as `_G.Bagnon`, rather than registering it as an AceAddon. The lookup
therefore failed, so ShardSource could paint the Blizzard bag buttons but not
Bagnon's item buttons.

### Fix

Use the global instance provided by this Bagnon build:

```lua
local bagnon = _G.Bagnon
```

Keep the existing checks for `bagnon` and `bagnon.ItemGroup`, the `Layout` hook,
and the loop that paints currently open Bagnon item groups.

### Reapplying after an upstream update

1. Open `ShardSource.lua` and find `local function RefreshBagnon()`.
2. Replace the lookup inside it with `local bagnon = _G.Bagnon`.
3. Confirm the following guard remains:

   ```lua
   if not bagnon or not bagnon.ItemGroup then return end
   ```

4. Reload the UI or restart the client. Check that ShardSource loads and that
   Soul Shard borders appear in Bagnon.

The current installation already includes this fix. The addon metadata version
and tracking revision were also advanced for that release; the functional
compatibility patch is the Bagnon lookup above.

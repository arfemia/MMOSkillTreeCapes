# MMO Skill Capes — Standalone Asset Pack

A pure Hytale asset pack that adds 20 cosmetic mastery capes — one per skill, plus a Master Cape for max total level. No Java code, no gameplay logic; the items render and exist on any Hytale server that loads this pack.

## What's included

- **20 cape items** (`Server/Item/Items/Armor/Capes/Cape_*.json`):
  - Cape_Master + 19 skill capes (Acrobatics, Archery, Artillery, Axes, Blunt, Building, Crafting, Daggers, Defense, Excavation, Fishing, Harvesting, Magic, Mining, Polearms, Staves, Swords, Unarmed, Woodcutting).
- **Custom rarity** `MMO_Skill_Cape` (`Server/Item/Qualities/MMO_Skill_Cape.json`) — gold tooltip, custom slot/tooltip frames, "Drop_Legendary" particles.
- **Localized names + descriptions** for all 9 supported locales (en-US, de-DE, es-ES, fr-FR, hu-HU, it-IT, pt-BR, ru-RU, tr-TR).
- **Shared model** `Cape_Skill.blockymodel` plus per-cape PNG textures, inventory icons, and quality UI textures.

## Granting capes

This pack only declares the items — it does not grant them. Three options:

1. **Use the [MMO Skill Tree plugin](https://wintergreen-solutions.com)** — its `command-rewards.json` automatically gives the matching cape on reaching level 100 in each skill (and level 1500 total for Cape_Master). If you install the plugin, do **not** also install this pack — see "Coexistence" below.
2. **Admin grant** — `give <player> Cape_Skill_Mining --quantity=1` from the console.
3. **Custom logic** — wire your own plugin/script to issue the give command on whatever condition you like.

## Coexistence with MMO Skill Tree

The [MMO Skill Tree plugin](https://wintergreen-solutions.com) bundles **the same** cape assets internally (`IncludesAssetPack: true`). If you install both this standalone pack AND that plugin on the same server, Hytale will see duplicate item / quality / translation IDs and warn at load.

**Install one or the other, not both:**
- Want gameplay + capes? Install only the plugin.
- Want capes only (e.g., for cosmetic-only servers)? Install only this pack.

## Known asset gaps

Three cape JSONs ship without their texture PNG: `Cape_Skill_Fishing`, `Cape_Skill_Artillery`, `Cape_Skill_Magic`. The items work — they just render with Hytale's missing-texture fallback until art is provided.

## Build (from source)

```powershell
.\build.ps1                  # build the zip, and install it if a Mods folder is known
.\build.ps1 -Install:$false  # build only, no copy
```

The script is self-contained and cross-platform (`pwsh ./build.ps1` works on macOS/Linux). It zips with the forward-slash plus directory entries Hytale needs; never use `Compress-Archive`. To auto-install on build, set `HYTALE_MODS_DIR` once to your Hytale `UserData/Mods` folder (or pass `-ModsDir <path>`); without it the script just builds the zip.

## Sync rule

This pack is the source-of-truth mirror of the cape assets in the parent MMO Skill Tree plugin (`src/main/resources/` in the monorepo). When cape art, item defs, the `MMO_Skill_Cape` quality, or cape translation strings change in either place, copy the change across both. The `manifest.json` here intentionally omits the plugin-only fields (`Main`, `Permissions`, `IncludesAssetPack`).

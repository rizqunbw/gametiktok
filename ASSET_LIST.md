# KINGDOM CHAOS — 3D Asset Procurement v1

Updated 2026-11-12. **This is an asset manifest, not a claim that model binaries are installed.** Free tiers only; verify the downloaded archive and included license before committing.

| Priority | Pack | Usage | Format | License shown by creator | Download |
|---|---|---|---|---|---|
| P0 | KayKit Adventurers FREE | Soldier, knight, 5 hero bases, weapons | FBX, glTF | CC0 | https://kaylousberg.itch.io/kaykit-adventurers |
| P0 | KayKit Character Animations FREE | idle, run, hit, death, melee, ranged, spawn | FBX, glTF | CC0 | https://kaylousberg.itch.io/kaykit-character-animations |
| P0 | Kenney Castle Kit | castle, gates, towers, siege | OBJ, FBX, glTF | CC0 | https://kenney.nl/assets/castle-kit |
| P1 | Quaternius Ultimate Animated Character Pack | NPC and monster variation; rigs differ | FBX, OBJ, Blend | CC0 **on this older pack page** | https://quaternius.com/packs/ultimatedanimatedcharacter |
| P1 | Quaternius RPG Characters | hero candidate, inspect per-pack terms | per download | VERIFY per pack | https://quaternius.com/packs/rpgcharacters.html |
| P2 | Poly Pizza | dragon/monster candidate discovery | varies | VERIFY per model | https://poly.pizza/explore |

## Recommended production mapping
- Red/Blue Soldier: reuse KayKit base rig, recolor team materials.
- Knight/Champion: KayKit models/accessories with character animation pack.
- Titan: scaled/stylized hero only after rig and combat animations checked.
- Castle: Kenney modular parts; identical geometry, two team palettes.
- Money Gun dragon: **not selected**. Must source rigged model with flying + 3 attack clips, verify license, inspect triangle count. Never substitute static dragon and claim it is animated.
- VFX: create original lightweight particle effects for Rose, Doughnut, Hand Heart and Dragon shots.

## License / repo safety
- KayKit free Adventurers and free Animations are CC0; paid EXTRA and SOURCE tiers are **not included**.
- Kenney Castle Kit CC0.
- Quaternius currently publishes QAL for assets released under QAL (https://quaternius.com/license.html), with restrictions on redistributing raw assets. The older Ultimate Animated pack page explicitly states CC0; keep evidence and license with exact archive. **Do not assume every Quaternius pack is CC0.**
- Poly Pizza: verify each model's author, license and attribution requirements individually.
- No mirrors or paid assets, no unverified scraped downloads. Do not push restricted raw asset packs into this public repository.
- For any third-party archive, record source URL, date, version, license file, checksum and whether redistribution is permitted.

## Acquisition checklist
- [ ] Download KayKit Adventurers **FREE** archive from official source
- [ ] Download KayKit Animations **FREE** archive
- [ ] Download Kenney Castle Kit official archive
- [ ] Validate archives, licenses, skeleton compatibility
- [ ] Convert selected assets to optimized .glb and test actual playback
- [ ] Find and vet a **rigged dragon** with fly/attack animations
- [ ] Profile 300–500 units on target PC
- [ ] Only then mark assets **installed**

## Suggested project folders
```
assets/
  README.md
  source/          # local downloads; not committed by default
  optimized/       # vetted, licensed runtime GLB
  licenses/        # attribution / license notes per source
  vfx/             # original effects
```

## Technical acceptance
For the core crowd, share mesh/material where possible, use animation LOD, GPU instancing or animation textures, cull off-screen units, cap active visible actors, compress textures, and verify smooth multi-hit combat. 500 independent skinned actors can be expensive even with low-poly models.

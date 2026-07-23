# Agent WebP Processing

Agent images are stored as WebP files directly in this directory. Use an existing agent, such as `110`, as the naming and sizing reference.

## Output Files

For agent `<id>`, create these files:

| File | Dimensions | Source image |
| --- | --- | --- |
| `<id>.webp` | 610x1080 | Achievement portrait |
| `<id>.icon.webp` | 256x256 | HUD portrait |
| `<id>_rounded.icon.webp` | 128x128 | Minimap portrait |
| `<id>_sm.icon.webp` | 100x100 | Cropped HUD portrait |
| `<id>_thin.icon.webp` | 112x64 | Scoreboard portrait |

## Requirements

- Encode as WebP with alpha preserved and quality `100`.
- Resize the full achievement portrait to `610x1080`.
- Do not resize the HUD, minimap, or scoreboard source images when they already match the output dimensions.
- Create the small icon from the HUD portrait, not the minimap portrait.
- Match the reference small-icon framing with a `196x196` crop at offset `+27+4`, then resize it to `100x100`.

## Commands

Run these commands with ImageMagick installed. Replace `<id>` and source filenames as appropriate:

```sh
magick assets/agents/<id>/T_Dynamic_RoleAchievementPortrait_<id>.png \
  -resize 610x1080! -quality 100 assets/agents/<id>.webp
magick assets/agents/<id>/T_Dynamic_RoleHUD_<id>.png \
  -quality 100 assets/agents/<id>.icon.webp
magick assets/agents/<id>/T_Dynamic_RoleMinimap_<id>.png \
  -quality 100 assets/agents/<id>_rounded.icon.webp
magick assets/agents/<id>/T_Dynamic_RoleHUD_<id>.png \
  -crop 196x196+27+4 +repage -resize 100x100! -quality 100 \
  assets/agents/<id>_sm.icon.webp
magick assets/agents/<id>/T_Dynamic_RoleScoreboard_<id>.png \
  -quality 100 assets/agents/<id>_thin.icon.webp
```

## Validation

Verify the generated files after conversion:

```sh
identify -format "%f %wx%h %[channels] %m %Q\\n" \
  assets/agents/<id>.webp \
  assets/agents/<id>.icon.webp \
  assets/agents/<id>_rounded.icon.webp \
  assets/agents/<id>_sm.icon.webp \
  assets/agents/<id>_thin.icon.webp
```

Expected output is RGBA WebP at quality `100`, with the dimensions listed above.
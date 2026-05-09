---
name: codex-pet-maker
description: Create, repair, package, and install Codex custom pets from photos, memes, character references, or generated spritesheets. Use when the user wants to make a Codex desktop pet, fix pet ghosting/cropping, correct movement direction, build an 8x9 spritesheet, write pet.json, or install a pet into ~/.codex/pets.
---

# Codex Pet Maker

Use this skill to produce Codex-compatible custom pets. A valid pet package is a folder under `~/.codex/pets/<pet-folder>/` containing:

- `pet.json`
- `spritesheet-clean.png`
- optionally `spritesheet.png` and QA files

The current Codex pet renderer expects an `8 x 9` atlas at `1536 x 1872`, sliced into `192 x 208` cells.

## Row Contract

Keep this row order unless the local renderer is known to use a different contract:

1. Idle / standing or sitting
2. Left movement: the pet must face left
3. Right movement: the pet must face right
4. Greet / attention
5. Happy / jump / play
6. Error / confused
7. Progress bars: `0%`, `20%`, `40%`, `60%`, `80%`, `100%`, `100%`, `100%`
8. Front movement / patrol
9. Coding / laptop / keyboard

Important: if a user reports that left/right movement faces the wrong way, flip only rows 2 and/or 3 per cell with `scripts/pet_atlas.py flip-rows`. Do not mirror the entire row across columns; that reverses frame order.

Direction repair table:

- Only left movement faces wrong: `flip-rows --rows 2`.
- Only right movement faces wrong: `flip-rows --rows 3`.
- Both movement rows face wrong: `flip-rows --rows 2 3`.
- Direction is correct but animation plays backward: do not flip; restore row frame order or regenerate the row.

## Workflow

1. Inspect existing pets first if the user says "like the previous one": `find ~/.codex/pets -maxdepth 2 -type f`.
2. Do not draw final artwork with programmatic vector/geometric scripts. Use image generation for final mascot art; keep scripts for packaging, cleanup, validation, direction fixes, and occasional bad-cell repair.
3. When a successful pet already exists, use its spritesheet as the style anchor. For cat-style dog pets, use `cream-orange-cat-coder` or `tuxedo-cat-coder` as the style reference, then use the user's dog photo as the identity reference.
4. Generate the spritesheet with the built-in image generation flow. Preserve the key identity traits but make a small, readable mascot.
5. Require one crisp subject per cell. Prompt against: ghosting, afterimages, motion trails, duplicate silhouettes, cropped ears/tails/paws, and cell bleeding.
6. Use the row contract. Do not replace movement rows with non-movement actions, even for animals; make the movement rows species-appropriate instead.
7. Save the generated image into the workspace, then run `scripts/pet_atlas.py package` to resize, remove fake checkerboard backgrounds, optionally repack components into fixed cells, create `pet.json`, and validate.
8. Open or inspect `spritesheet-grid-check.png`. Mechanical validation is necessary but not sufficient; visual QA catches bad style, missing cells, cell bleeding, and left/right movement direction errors.
9. Install by copying the package contents into `~/.codex/pets/<pet-id>/`. If the target folder already exists, use `cp -R <pet-dir>/. ~/.codex/pets/<pet-id>/`; do not copy the folder onto itself or Codex may keep reading stale outer files.
10. If the settings UI still shows old art, tell the user to switch pets or restart Codex to clear cache.

## Recommended Generation Prompt Shape

Use the templates in `references/prompt-templates.md` when starting from scratch. Choose the pig template for glossy chibi toy mascots, the cat template for pet-photo cat mascots, and the dog template for cat-style soft pet-photo dog mascots.

For dogs, use the existing cat pets as the style anchor: delicate fur edges, soft shading, rounded semi-3D sticker volume, and cute expressive faces. Use the dog photo only as the identity anchor for breed traits, markings, ears, tail, proportions, and expression. Do not use program-drawn final art, pixel art, flat emoji, hard vector outlines, or geometric block bodies unless the user explicitly asks for that style.

Always include:

```text
Canvas: exactly 1536x1872 if possible, 8 columns x 9 rows, each frame fits a 192x208 cell.
Critical quality: one single subject per cell, no ghosting, no afterimages, no duplicate silhouettes, no motion trails, no cropped limbs/ears/tail/props, no cell bleeding.
Background: transparent if possible, otherwise flat removable light checker-free background.
```

## Deterministic Tools

Run scripts from the skill directory:

```bash
python scripts/pet_atlas.py package \
  --input <generated.png> \
  --out-dir <pet-folder> \
  --id <pet-id> \
  --display-name "<Display Name>" \
  --description "<Description>" \
  --repack
```

If the local `python` does not include Pillow, call `load_workspace_dependencies` in Codex and use the bundled Python executable it returns.

Useful commands:

```bash
python scripts/pet_atlas.py validate --pet-dir <pet-folder>
python scripts/pet_atlas.py flip-rows --pet-dir <pet-folder> --rows 2 3
python scripts/pet_atlas.py install --pet-dir <pet-folder>
```

Use `--repack` when the generated atlas visually looks like a grid but Codex previews show fragments from neighboring cells. Repacking labels whole connected components and re-centers them into true `192 x 208` cells. Always inspect `spritesheet-grid-check.png` afterward; if a frame is missing, replace the bad cell from a neighboring good frame or regenerate that row.

Use `flip-rows` when movement direction is reversed. It flips each frame inside the listed row numbers while preserving frame order.

Before finishing, always test or inspect the movement contract: row 2 faces left and row 3 faces right. If the local Codex preview still shows old movement, reinstall the pet folder contents into `~/.codex/pets/<pet-id>/` and restart Codex to clear cache.

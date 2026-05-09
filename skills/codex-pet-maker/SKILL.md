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
2. Movement direction A
3. Movement direction B
4. Greet / attention
5. Happy / jump / play
6. Error / confused
7. Progress bars: `0%`, `20%`, `40%`, `60%`, `80%`, `100%`, `100%`, `100%`
8. Front movement / patrol
9. Coding / laptop / keyboard

Important: if a user reports that left/right movement faces the wrong way, flip only rows 2 and/or 3 per cell with `scripts/pet_atlas.py flip-rows`. Do not mirror the entire row across columns; that reverses frame order.

## Workflow

1. Inspect existing pets first if the user says "like the previous one": `find ~/.codex/pets -maxdepth 2 -type f`.
2. Generate the spritesheet with the built-in image generation flow. If the user provides reference images, preserve the key identity traits but make a small, readable mascot.
3. Require one crisp subject per cell. Prompt against: ghosting, afterimages, motion trails, duplicate silhouettes, cropped ears/tails/paws, and cell bleeding.
4. Use the row contract. Do not replace movement rows with non-movement actions, even for animals; make the movement rows species-appropriate instead.
5. Save the generated image into the workspace, then run `scripts/pet_atlas.py package` to resize, remove fake checkerboard backgrounds, optionally repack components into fixed cells, create `pet.json`, and validate.
6. Install by copying the package folder into `~/.codex/pets/`. In Codex sandboxed environments this may require user approval.
7. If the settings UI still shows old art, tell the user to switch pets or restart Codex to clear cache.

## Recommended Generation Prompt Shape

Use the templates in `references/prompt-templates.md` when starting from scratch. Choose the pig template for glossy chibi toy mascots, the cat template for pet-photo cat mascots, and the dog template for soft pet-photo dog mascots.

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

Use `--repack` when the generated atlas visually looks like a grid but Codex previews show fragments from neighboring cells. Repacking labels whole connected components and re-centers them into true `192 x 208` cells.

Use `flip-rows` when movement direction is reversed. It flips each frame inside the listed row numbers while preserving frame order.

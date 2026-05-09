# 0 基础：跟着做一组 Codex 哈士奇宠物

这篇文档记录 `pets/husky-coder` 的制作过程。目标是把参考图里的哈士奇做成和猫咪宠物一致的细腻软萌插画风，而不是像素画、硬边图标或扁平矢量图。

## 目标效果

最终文件夹：

```text
pets/husky-coder/
├── pet.json
├── spritesheet.png
├── spritesheet-clean.png
└── spritesheet-grid-check.png
```

哈士奇特征：

- 黑白面罩和白色脸颊。
- 直立尖耳。
- 参考图里的明亮眼睛，本组做成一蓝一棕，方便小尺寸辨认。
- 活泼张嘴、粉色舌头、卷尾。
- 最终美术用图片生成完成；程序脚本只负责打包、清理、校验和少量坏格修补，不用来从零画角色。

## 制作步骤

1. 调用 skill：

```text
用 $codex-pet-maker 根据哈士奇参考图做一个软萌风 Codex 宠物，放到 pets/husky-coder。
```

2. 生成时锁定这些要求：

```text
Style anchor: use the existing cat pet spritesheet as the visual anchor, with delicate fur edges, soft semi-3D sticker look, gentle shading, subtle fur texture.
Identity anchor: use the husky photo for black-and-white mask, upright ears, bright eyes, lively expression, and curled tail.
Avoid pixel art, hard vector icon outlines, flat emoji style, jagged edges, and blocky shapes.
Rows: idle, left movement, right movement, greeting, happy jump, error/confused, progress, front walk, laptop coding.
Critical quality: one husky per cell, no ghosting, no afterimages, no cropped ears/tail/paws, no cell bleeding.
```

3. 打包：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py package \
  --input work/dogs/husky-soft-atlas.png \
  --out-dir pets/husky-coder \
  --id husky-coder \
  --display-name "Husky Coder" \
  --description "A bright-eyed soft-style husky coding companion."
```

4. 校验：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py validate --pet-dir pets/husky-coder
```

打开 `pets/husky-coder/spritesheet-grid-check.png`，重点检查尖耳和卷尾不要贴边，侧跑两行不要串格。

## 常见问题

- 有残影：打包时加 `--repack`。
- 左右方向反：运行 `python skills/codex-pet-maker/scripts/pet_atlas.py flip-rows --pet-dir pets/husky-coder --rows 2 3`。
- 眼睛太小看不出品种：放大眼睛高光或强化面罩，不要加复杂背景。

## 安装

```bash
mkdir -p ~/.codex/pets
cp -R pets/husky-coder ~/.codex/pets/husky-coder
```

安装后选择 `Husky Coder`。

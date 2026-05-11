# 0 基础：跟着做一组 Codex 金毛宠物

这篇文档记录 `pets/golden-retriever-coder` 的制作过程。目标是把参考图里的金毛做成接近猫咪范本的软萌插画风宠物，温柔、长毛感明显，同时避免程序矢量感。

## 目标效果

最终文件夹：

```text
pets/golden-retriever-coder/
├── pet.json
├── spritesheet.png
├── spritesheet-clean.png
└── spritesheet-grid-check.png
```

金毛特征：

- 金色长毛和柔和胸毛。
- 下垂耳朵。
- 温柔圆眼、黑鼻子、开心张嘴。
- 蓬松尾巴，侧面动作里要能看出长毛狗轮廓。
- 最终美术用图片生成完成；程序脚本只负责打包、清理、校验和少量坏格修补，不用来从零画角色。

## 制作步骤

1. 调用 skill：

```text
用 $codex-pet-maker 根据金毛参考图做一个软萌风 Codex 宠物，放到 pets/golden-retriever-coder。
```

2. 生成时锁定这些要求：

```text
Style anchor: use the existing cat pet spritesheet as the visual anchor, with delicate long-fur edges, soft semi-3D sticker look, gentle shading, warm highlights.
Identity anchor: use the golden retriever photo for golden coat, droopy ears, gentle round eyes, black nose, long fur, and fluffy tail.
Avoid pixel art, hard vector icon outlines, flat emoji style, jagged edges, and blocky shapes.
Rows: idle, right movement, left movement, greeting, happy jump, error/confused, progress, front walk, laptop coding.
Critical quality: one golden retriever per cell, no ghosting, no afterimages, no cropped ears/tail/paws, no cell bleeding.
```

3. 打包：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py package \
  --input work/dogs/golden-soft-atlas.png \
  --out-dir pets/golden-retriever-coder \
  --id golden-retriever-coder \
  --display-name "Golden Retriever Coder" \
  --description "A gentle soft-style golden retriever coding companion."
```

4. 校验：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py validate --pet-dir pets/golden-retriever-coder
```

打开 `pets/golden-retriever-coder/spritesheet-grid-check.png`，重点检查尾巴和耳朵不要贴边，进度条文字要清楚。

## 常见问题

- 长毛导致轮廓糊：减少细碎毛发，保留柔和高光和大轮廓。
- 有残影：打包时加 `--repack`。
- 左右方向反：运行 `python skills/codex-pet-maker/scripts/pet_atlas.py flip-rows --pet-dir pets/golden-retriever-coder --rows 2 3`。当前 Codex renderer 中第 2 行应面向右，第 3 行应面向左。

## 安装

```bash
mkdir -p ~/.codex/pets
cp -R pets/golden-retriever-coder ~/.codex/pets/golden-retriever-coder
```

安装后选择 `Golden Retriever Coder`。

# 0 基础：跟着做一组 Codex 狸花猫宠物

这篇文档记录 `pets/lihua-cat-coder` 的制作过程。目标是用狸花猫参考图生成一组和现有猫咪宠物一致的软萌半立体贴纸风 Codex pet。

## 目标效果

最终文件夹：

```text
pets/lihua-cat-coder/
├── pet.json
├── spritesheet.png
├── spritesheet-clean.png
└── spritesheet-grid-check.png
```

狸花猫特征：

- 棕金色虎斑毛色。
- 额头深色 M 纹。
- 脸颊、腿部和尾巴有清晰条纹。
- 大而圆的琥珀眼，小粉鼻子。
- 整体保持软萌、毛茸茸、半立体贴纸感。

## 制作步骤

1. 调用 skill：

```text
用 $codex-pet-maker 根据这张狸花猫参考图做一个 Codex 宠物。
```

2. 生成时锁定这些要求：

```text
Final artwork must be image-generated, not programmatic vector drawing.
Style anchor: match the existing soft cat pet spritesheets: fluffy fur edges, gentle semi-3D sticker volume, soft shadows, bright round eyes, cute rounded proportions.
Identity anchor: brown and golden mackerel tabby stripes, dark forehead M marking, striped legs and tail, pale muzzle, small pink nose, alert triangular ears, curious expression.
Rows: idle, right-facing side walk, left-facing side walk, attention, cat play, error/confused, progress, front walk, laptop coding.
Critical quality: one cat per cell, no ghosting, no afterimages, no cropped ears/tail/paws, no cell bleeding.
```

3. 打包：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py package \
  --input work/lihua-cat/lihua-cat-atlas.png \
  --out-dir pets/lihua-cat-coder \
  --id lihua-cat-coder \
  --display-name "狸花猫 Coder" \
  --description "A soft tabby cat coding companion." \
  --repack
```

4. 方向检查：

打开 `pets/lihua-cat-coder/spritesheet-grid-check.png`，必须看实际像素：

- 第 2 行必须面向右。
- 第 3 行必须面向左。
- 如果第 2 行被模型画成朝左，只翻第 2 行：`flip-rows --rows 2`。
- 如果两行都反，再翻第 2/3 行：`flip-rows --rows 2 3`。

5. 格线检查：

- 耳朵、尾巴、爪子、电脑、进度条不能贴住红色格线。
- 有小杂点时只清理透明小组件，不重新生成整版美术。

6. 校验：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py validate --pet-dir pets/lihua-cat-coder
```

## 安装

```bash
mkdir -p ~/.codex/pets/lihua-cat-coder
cp -R pets/lihua-cat-coder/. ~/.codex/pets/lihua-cat-coder/
```

如果 Codex 仍显示旧图或旧方向，重启 Codex 清缓存。

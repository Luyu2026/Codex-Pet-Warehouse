# 0 基础：跟着做一组 Codex 兔子宠物

这篇文档记录 `pets/rabbit-coder` 的制作过程。兔子这类白色主体有一个特殊坑：不能直接用白底或棋盘格底做最终清理，否则白毛容易被误删。

## 目标效果

最终文件夹：

```text
pets/rabbit-coder/
├── pet.json
├── spritesheet.png
├── spritesheet-clean.png
└── spritesheet-grid-check.png
```

兔子特征：

- 白色毛茸茸身体。
- 高高竖起的耳朵，耳朵内侧淡粉色。
- 圆眼睛、小粉鼻子、小爪子。
- 整体保持和猫咪宠物接近的软萌半立体贴纸风。

## 制作步骤

1. 调用 skill：

```text
用 $codex-pet-maker 根据这张白色兔子参考图做一个 Codex 桌面宠物，名字叫 Rabbit Coder。
```

2. 生成时锁定这些要求：

```text
Final artwork must be image-generated, not programmatic vector drawing.
Style anchor: match the existing soft cat pet spritesheets: fluffy fur edges, gentle semi-3D sticker volume, soft shadows, bright round eyes, cute rounded proportions.
Identity anchor: white fluffy rabbit, tall upright ears with pink inner ears, tiny pink nose, compact body, small paws, curious expression.
Background: transparent if possible. If transparency is unreliable, use a flat removable pale cyan/blue background, not white and not checkerboard.
Rows: idle, right-facing side hop, left-facing side hop, attention, happy hop, error/confused, progress, front hop, laptop coding.
Critical quality: one rabbit per cell, no ghosting, no afterimages, no cropped ears/paws, no cell bleeding.
```

3. 打包：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py package \
  --input work/rabbit/rabbit-atlas.png \
  --out-dir pets/rabbit-coder \
  --id rabbit-coder \
  --display-name "Rabbit Coder" \
  --description "A fluffy white rabbit coding companion." \
  --repack
```

4. 方向检查：

打开 `pets/rabbit-coder/spritesheet-grid-check.png`：

- 第 2 行必须面向右。
- 第 3 行必须面向左。
- 如果两行都反，用 `flip-rows --rows 2 3`。
- 只修朝向时按单格镜像，不倒序整行。

5. 白毛检查：

- 在白底格线图里看有没有串格、缺格、裁切。
- 在深色背景里看白毛边缘有没有被抠掉。
- 如果白毛被擦掉，重新从浅青/浅蓝背景版本抠图，不要继续用白底版本。

6. 校验：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py validate --pet-dir pets/rabbit-coder
```

## 安装

```bash
mkdir -p ~/.codex/pets/rabbit-coder
cp -R pets/rabbit-coder/. ~/.codex/pets/rabbit-coder/
```

安装后选择 `Rabbit Coder`。如果 Codex 仍显示旧图或旧方向，重启 Codex 清缓存。

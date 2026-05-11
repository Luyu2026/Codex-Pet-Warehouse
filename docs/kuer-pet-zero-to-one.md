# 0 基础：跟着做一组 Codex 酷儿宠物

这篇文档记录 `pets/酷儿-coder` 的制作过程。目标是把用户提供的蓝色卡通形象做成 Codex 宠物，同时遵守本仓库沉淀下来的两个关键原则：最终美术用图片生成完成；当前 Codex renderer 中第 2 行必须右跑，第 3 行必须左跑。

## 目标效果

最终文件夹：

```text
pets/酷儿-coder/
├── pet.json
├── spritesheet.png
├── spritesheet-clean.png
└── spritesheet-grid-check.png
```

酷儿特征：

- 天蓝色圆头和小身体。
- 左侧尖尖的猫耳轮廓。
- 黑色粗描边。
- 橙色圆脸颊、黑色椭圆鼻子。
- 开心眯眼、吐舌头、双手合十的可爱表情。

## 制作步骤

1. 调用 skill：

```text
用 $codex-pet-maker 根据这张蓝色卡通形象做一个 Codex 宠物，名字叫酷儿。
```

2. 生成时锁定这些要求：

```text
Final artwork must be image-generated, not programmatic vector drawing.
Style anchor: preserve the reference character's clean black outline, flat friendly colors, expressive face, and readable silhouette.
Identity anchor: sky-blue body, round head, one pointed cat-like ear, orange cheek circles, black oval nose, closed smiling eyes, tiny body, clasped hands.
Rows: idle, right movement, left movement, greeting, happy bounce, error/confused, progress, front walk, laptop coding.
Critical quality: one mascot per cell, no ghosting, no afterimages, no cropped ear/limbs/props, no cell bleeding.
```

3. 打包：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py package \
  --input work/kuer/kuer-atlas.png \
  --out-dir pets/酷儿-coder \
  --id 酷儿-coder \
  --display-name "酷儿" \
  --description "A cheerful blue cartoon mascot coding companion." \
  --repack
```

4. 方向检查：

打开 `pets/酷儿-coder/spritesheet-grid-check.png`：

- 第 2 行必须面向右。
- 第 3 行必须面向左。
- 如果两行都反，用 `flip-rows --rows 2 3`。
- 如果只有右移动反，用 `flip-rows --rows 2`。
- 如果只有左移动反，用 `flip-rows --rows 3`。

5. 校验：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py validate --pet-dir pets/酷儿-coder
```

## 安装

```bash
mkdir -p ~/.codex/pets/酷儿-coder
cp -R pets/酷儿-coder/. ~/.codex/pets/酷儿-coder/
```

安装后选择 `酷儿`。如果 Codex 仍显示旧图或旧方向，重启 Codex 清缓存。

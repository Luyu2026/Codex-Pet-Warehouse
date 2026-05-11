# 0 基础：跟着做一组 Codex 柯基宠物

这篇文档记录 `pets/corgi-coder` 的重做过程。旧版柯基轮廓偏硬、像矢量图，和仓库里两只猫咪的软萌宠物风不统一；新版以猫咪 spritesheet 为风格锚点，重做成细腻毛边、柔和阴影、半立体贴纸感。

## 目标效果

最终文件夹：

```text
pets/corgi-coder/
├── pet.json
├── spritesheet.png
├── spritesheet-clean.png
└── spritesheet-grid-check.png
```

画风标准：

- 圆润 chibi 比例，接近两只猫咪 pet 的细腻插画感，不要像素画、硬边矢量图或扁平图标。
- 保留柯基的大立耳、棕白脸、白色鼻梁、短腿和开心表情。
- 每格只有一个主体，透明背景，无重影、无串格、无裁切。
- 最终美术用图片生成完成；程序脚本只负责打包、清理、校验和少量坏格修补，不用来从零画角色。

## 制作步骤

1. 安装并调用 skill：

```text
用 $codex-pet-maker 根据柯基参考图做一个软萌风 Codex 宠物，覆盖 pets/corgi-coder。
```

2. 按 8x9 动作表生成 spritesheet：

生成时把两类参考分开：猫咪 spritesheet 负责风格，柯基照片负责身份。这样可以避免做成程序矢量图或普通图标。

| 行数 | 动作 |
| --- | --- |
| 1 | 坐姿待机、眨眼、呼吸 |
| 2 | 向右侧跑，短腿交替 |
| 3 | 向左侧跑，短腿交替 |
| 4 | 抬爪打招呼 |
| 5 | 开心跳跃 |
| 6 | 报错和困惑 |
| 7 | 进度条：`0%`、`20%`、`40%`、`60%`、`80%`、`100%`、`100%`、`100%` |
| 8 | 正面走动 |
| 9 | 坐着用小电脑写代码 |

3. 打包：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py package \
  --input work/dogs/corgi-soft-atlas.png \
  --out-dir pets/corgi-coder \
  --id corgi-coder \
  --display-name "Corgi Coder" \
  --description "A cheerful soft-style corgi coding companion with big ears and tiny paws."
```

4. 校验：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py validate --pet-dir pets/corgi-coder
```

看到 `OK pets/corgi-coder` 即基础格式正确。再打开 `spritesheet-grid-check.png` 检查耳朵、爪子、电脑、进度条是否都在格子内。

## 常见问题

- 如果有残影或串格，重新打包时加 `--repack`。
- 如果左右移动方向反了，运行 `python skills/codex-pet-maker/scripts/pet_atlas.py flip-rows --pet-dir pets/corgi-coder --rows 2 3`。当前 Codex renderer 中第 2 行应面向右，第 3 行应面向左。
- 如果 Codex 里仍显示旧图，重新打开宠物列表；必要时切换一次宠物或重启 Codex。

## 安装

```bash
mkdir -p ~/.codex/pets
cp -R pets/corgi-coder ~/.codex/pets/corgi-coder
```

安装后选择 `Corgi Coder`。

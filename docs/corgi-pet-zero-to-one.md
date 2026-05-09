# 0 基础：跟着做一组 Codex 柯基宠物

这篇文档记录本仓库里 `pets/corgi-coder` 的制作过程。目标是让没有做过 Codex 宠物的人，也能照着完成一组可安装、可校验、可排错的自定义宠物。

## 你会做出什么

最终产物是一个宠物文件夹：

```text
pets/corgi-coder/
├── pet.json
├── spritesheet.png
├── spritesheet-clean.png
└── spritesheet-grid-check.png
```

其中：

- `pet.json` 是 Codex 识别宠物用的配置。
- `spritesheet-clean.png` 是真正展示的透明精灵图。
- `spritesheet.png` 是兼容保留文件。
- `spritesheet-grid-check.png` 是带红色格线的检查图，用来确认有没有裁切、串格、重影。

## 第 1 步：准备参考图

这次参考图是一只正面坐着的柯基。提炼出来的关键特征是：

- 大立耳。
- 棕白脸，额头到鼻梁有白色竖纹。
- 黑鼻子、黑亮眼睛。
- 开心张嘴、粉色舌头。
- 短腿、圆润小体型。

做宠物时不要追求照片级复刻，先保证“小尺寸预览里一眼能认出来”。Codex 宠物实际显示很小，清晰轮廓比细节更重要。

## 第 2 步：调用 skill

安装本仓库的 skill 后，可以在 Codex 里这样说：

```text
用 $codex-pet-maker 根据这张柯基参考图做一个 Codex 宠物，放到 pets/corgi-coder，并生成新手教程文档。
```

这个 skill 会约束宠物必须是：

- `8` 列 x `9` 行。
- 整张图 `1536 x 1872`。
- 每格 `192 x 208`。
- 透明背景。
- 每格只有一个主体，不能有重影、拖影、半个身体漏进隔壁格子。

## 第 3 步：规划 9 行动作

本仓库采用下面的动作约定：

| 行数 | 动作 |
| --- | --- |
| 1 | 待机，坐着、眨眼、呼吸 |
| 2 | 向一个方向侧跑 |
| 3 | 向另一个方向侧跑 |
| 4 | 打招呼、抬爪 |
| 5 | 开心、跳跃 |
| 6 | 报错、困惑 |
| 7 | 进度条：`0%`、`20%`、`40%`、`60%`、`80%`、`100%`、`100%`、`100%` |
| 8 | 正面走动 |
| 9 | 坐在电脑前写代码 |

柯基这一组特别注意：侧跑时腿短，所以动作幅度不能太大，否则很容易被单格边缘裁掉。

## 第 4 步：生成原始 spritesheet

可以用图片生成模型生成，也可以像本次一样先生成一张干净的透明原始图。无论哪种方式，原始图都应该接近：

```text
1536 x 1872
8 columns x 9 rows
192 x 208 per cell
transparent background
```

如果用图片生成模型，提示词里一定要写清楚：

```text
Create a Codex-compatible 8-column by 9-row chibi corgi spritesheet from the reference dog photo.
Preserve key traits: big upright ears, tan-and-white face, white blaze from forehead to snout, black nose, happy open mouth, pink tongue, tiny short legs.
Canvas: exactly 1536x1872 if possible, each frame fits a 192x208 cell.
Rows:
1 seated idle, blink, breathing;
2 side-run movement facing one direction, tiny legs alternating;
3 side-run movement facing the opposite direction, tiny legs alternating;
4 paw raise / greeting;
5 happy jump;
6 error/confused, first two frames holding ERROR sign;
7 progress bars labeled exactly 0%, 20%, 40%, 60%, 80%, 100%, 100%, 100%;
8 front-facing walk toward viewer;
9 sitting with small dark laptop, typing/blinking.
Critical quality: one corgi per cell, no ghosting, no afterimages, no duplicate silhouettes, no motion trails, no shadows, no cropped ears/paws, no cell bleeding.
Background: transparent if possible, otherwise flat removable light checker-free background.
```

## 第 5 步：用脚本打包

在仓库根目录运行：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py package \
  --input work/corgi/corgi-raw-atlas.png \
  --out-dir pets/corgi-coder \
  --id corgi-coder \
  --display-name "Corgi Coder" \
  --description "A cheerful corgi coding companion with big ears and tiny paws."
```

如果系统 `python3` 没有 Pillow，在 Codex 里调用 `load_workspace_dependencies`，然后改用返回的 bundled Python，例如：

```bash
/Users/admin/.cache/codex-runtimes/codex-primary-runtime/dependencies/python/bin/python3 \
  skills/codex-pet-maker/scripts/pet_atlas.py validate \
  --pet-dir pets/corgi-coder
```

## 第 6 步：校验

运行：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py validate --pet-dir pets/corgi-coder
```

看到下面结果就说明基础格式没问题：

```text
OK pets/corgi-coder
```

然后打开 `pets/corgi-coder/spritesheet-grid-check.png` 看红色格线：

- 每个格子里只能有一个柯基。
- 耳朵、爪子、电脑、进度条不能贴边被裁。
- 隔壁格子不能出现上一帧残影。
- 第 7 行的百分比文字要能读出来。

## 常见问题

### 有重影或隔壁格子漏进来

重新打包时加 `--repack`：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py package \
  --input work/corgi/corgi-raw-atlas.png \
  --out-dir pets/corgi-coder \
  --id corgi-coder \
  --display-name "Corgi Coder" \
  --description "A cheerful corgi coding companion with big ears and tiny paws." \
  --repack
```

### 左右移动方向反了

翻转第 2、3 行里的每一格，不改变帧顺序：

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py flip-rows --pet-dir pets/corgi-coder --rows 2 3
```

注意：不要把整行左右倒序，那样会破坏动画帧顺序。

### Codex 里还是旧图

复制新文件后，重新打开自定义宠物列表；如果还没刷新，切换一次宠物或重启 Codex。

## 第 7 步：安装到 Codex

确认没问题后：

```bash
mkdir -p ~/.codex/pets
cp -R pets/corgi-coder ~/.codex/pets/corgi-coder
```

打开 Codex 的自定义宠物设置，选择 `Corgi Coder` 即可。

## 本次结果

本次已生成并提交：

- `pets/corgi-coder/pet.json`
- `pets/corgi-coder/spritesheet.png`
- `pets/corgi-coder/spritesheet-clean.png`
- `pets/corgi-coder/spritesheet-grid-check.png`

这组柯基采用干净透明图层和格线校验，避免了之前制作过程中遇到的重影、串格和方向难排查问题。

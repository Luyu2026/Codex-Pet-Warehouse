# Codex Pet 方向 QA 清单

这份清单专门防止宠物出现“往左移动却向右跑、往右移动却向左跑”的问题。

## 固定行约定

Codex 宠物 spritesheet 使用 8 列 x 9 行。当前 Codex desktop renderer 的实际移动行契约是：

- 第 2 行：右移动，角色必须面向右。
- 第 3 行：左移动，角色必须面向左。

这个契约看起来和早期文档相反，但以本机实际运行效果为准：如果第 2 行朝左、第 3 行朝右，Codex 会出现“向左移动却向右跑、向右移动却向左跑”。

修方向时只能镜像每个单格里的角色，不能把整行倒序。倒序会改变动画帧顺序，容易让跑步节奏变怪。

## 本次复盘出的根因

这次兔子和陆羽反复跑反，不是 `flip-rows` 工具失效，而是最终提交的实际 spritesheet 仍然是第 2 行朝左、第 3 行朝右；同时部分旧教程还写着早期的“第 2 行左、第 3 行右”，容易把后续制作带偏。后续统一以 `spritesheet-grid-check.png` 的实际像素为准，不能只看提示词或行名。

新的 `spritesheet-grid-check.png` 会直接标注：

- `row 2: RIGHT movement, character faces RIGHT`
- `row 3: LEFT movement, character faces LEFT`

如果实际图像和这两行标注不一致，先 `flip-rows --rows 2 3`，再安装和提交。

## 必做检查

打包完成后，先打开 `spritesheet-grid-check.png`：

1. 看第 2 行：每一格角色鼻子、脸、身体前进方向都应朝右。
2. 看第 3 行：每一格角色鼻子、脸、身体前进方向都应朝左。
3. 如果角色左右不明显，优先看脸、脚步、尾巴和动作重心。
4. 看每格是否贴住红色边界。贴边会让 Codex 小预览出现上一行/下一行碎片，看起来像重影。
5. 安装到本机后，在 Codex 里实际拖动测试：往左移动应向左跑，往右移动应向右跑。

## 修复决策表

| 观察到的问题 | 修复命令 |
| --- | --- |
| 只有左移动行朝错 | `python skills/codex-pet-maker/scripts/pet_atlas.py flip-rows --pet-dir <pet-dir> --rows 3` |
| 只有右移动行朝错 | `python skills/codex-pet-maker/scripts/pet_atlas.py flip-rows --pet-dir <pet-dir> --rows 2` |
| 左右两行都朝反 | `python skills/codex-pet-maker/scripts/pet_atlas.py flip-rows --pet-dir <pet-dir> --rows 2 3` |
| 方向对但跑步倒着播放 | 不要 flip，检查是否整行被倒序过；需要恢复正确帧顺序后重新打包 |

## 提交前要求

- 仓库目录执行 `validate`。
- 本机 `~/.codex/pets/<pet>` 执行 `validate`。
- 本机文件和仓库文件做一次哈希比对。
- 涉及 Git 提交或推送时，必须先确认仓库 `pets/<pet-id>` 和本机 `~/.codex/pets/<pet-id>` 已同步；不要只提交仓库而漏掉本机安装，也不要只改本机而漏掉 Git。
- 如果本机已经打开 Codex，刷新宠物列表或重启 Codex 清缓存。
- 覆盖本机已有宠物时，使用 `cp -R pets/<pet-id>/. ~/.codex/pets/<pet-id>/`，避免复制出同名嵌套目录。

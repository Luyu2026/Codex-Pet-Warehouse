# Codex Pet Warehouse

这里集结了 Codex 自定义宠物的制作流程、可复用 skill、以及本机已经做好的宠物包。

## 使用声明

本仓库内容仅供个人学习、研究和自用，请勿用于商业用途。部分范例宠物可能参考了第三方角色、公开图片或个人照片的视觉特征；使用者应自行确认相关素材和形象的授权边界，并避免分发、售卖或用于任何可能造成权利争议的场景。

## 目录

- `pets/`: 已完成的宠物包，复制到 `~/.codex/pets` 后即可在 Codex 的自定义宠物列表中选择。
- `skills/codex-pet-maker/`: 制作和修复 Codex 宠物的 skill，包含提示词范本、动作规划、重影修复、方向修复、打包校验脚本。

## 已收录宠物

- `pig-hero-coder`: 红衣小猪英雄风格宠物，按最终确认的动作组整理。
- `corgi-coder`: 基于柯基参考图制作的开心短腿编码伙伴。
- `cream-orange-cat-coder`: 奶油橘白猫宠物。
- `tuxedo-cat-coder`: 黑白长毛猫宠物。
- `陆羽-coder`: 基于真人照片生成的黑衣编码伙伴。

## 安装宠物

在仓库根目录执行：

```bash
mkdir -p ~/.codex/pets
cp -R pets/* ~/.codex/pets/
```

如果 Codex 已经打开，复制后重新打开自定义宠物列表；必要时切换一次宠物或重启 Codex，以避免缓存还停留在旧图。

## 安装 Skill

```bash
mkdir -p ~/.codex/skills
cp -R skills/codex-pet-maker ~/.codex/skills/codex-pet-maker
```

安装后，在 Codex 中可以这样调用：

```text
用 $codex-pet-maker 根据这张参考图做一个 Codex 宠物，并安装到自定义宠物文件夹。
```

## 制作流程摘要

1. 先确认宠物人格、名字、动作差异和参考图特征。
2. 用 skill 内的提示词范本生成 8 列 x 9 行的透明 spritesheet，尺寸为 `1536 x 1872`，单格 `192 x 208`。
3. 使用 `scripts/pet_atlas.py package` 清理假透明背景、可选重排格子、生成 `pet.json`。
4. 用 `scripts/pet_atlas.py validate` 校验尺寸、透明度和元数据。
5. 如移动方向相反，用 `scripts/pet_atlas.py flip-rows --rows 2 3` 按单格翻转方向行。
6. 用 `scripts/pet_atlas.py install` 安装到 `~/.codex/pets`。

脚本依赖 Pillow；如果系统 `python3` 没有 Pillow，可以在 Codex 里使用 bundled Python runtime。

更多提示词和排错细节见 `skills/codex-pet-maker/SKILL.md`。

## 新手教程

- [0 基础：跟着做一组 Codex 柯基宠物](docs/corgi-pet-zero-to-one.md)

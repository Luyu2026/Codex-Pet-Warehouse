# Codex Pet Skill

这里集结了 Codex 自定义宠物的制作流程、可复用 skill、以及本机已经做好的宠物包。


## 使用声明

本仓库内容仅供个人学习、研究和自用，请勿用于商业用途。部分范例宠物可能参考了第三方角色、公开图片或个人照片的视觉特征；使用者应自行确认相关素材和形象的授权边界，并避免分发、售卖或用于任何可能造成权利争议的场景。

## 目录

- `pets/`: 已完成的宠物包，复制到 `~/.codex/pets` 后即可在 Codex 的自定义宠物列表中选择。
- `skills/codex-pet-maker/`: 制作和修复 Codex 宠物的 skill，包含提示词范本、动作规划、重影修复、方向修复、打包校验脚本。
- `docs/`: 0 基础教程和制作经验记录，包含本次确认的美术流程结论。

## 已收录宠物

- `pig-hero-coder`: 红衣小猪英雄风格宠物，按最终确认的动作组整理。
- `corgi-coder`: 基于柯基参考图重做的猫咪范本软萌插画风短腿编码伙伴。
- `husky-coder`: 基于哈士奇参考图制作的猫咪范本软萌插画风编码伙伴。
- `golden-retriever-coder`: 基于金毛参考图制作的猫咪范本软萌插画风长毛编码伙伴。
- `cream-orange-cat-coder`: 奶油橘白猫宠物。
- `tuxedo-cat-coder`: 黑白长毛猫宠物。
- `陆羽-coder`: 基于真人照片生成的黑衣编码伙伴。
- `酷儿-coder`: 基于蓝色卡通形象制作的开心编码伙伴。
- `rabbit-coder`: 基于白色兔子参考图制作的毛茸茸编码伙伴。

## 安装宠物

批量安装全部宠物时，在仓库根目录执行：

```bash
mkdir -p ~/.codex/pets
cp -R pets/* ~/.codex/pets/
```

单独覆盖某个已存在宠物时，用复制“目录内容”的写法，避免套出 `pet-id/pet-id` 这种同名嵌套目录：

```bash
mkdir -p ~/.codex/pets/<pet-id>
cp -R pets/<pet-id>/. ~/.codex/pets/<pet-id>/
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

用户只需要给出参考图和这句需求；移动方向复核、打包校验和本机安装是 `codex-pet-maker` skill 的内置流程。

## 制作流程摘要

1. 先确认宠物人格、名字、动作差异和参考图特征。
2. 最终美术使用图片生成流程，不用程序脚本从零绘制。程序只负责打包、清理、校验、方向修复和少量补坏格。
3. 用已经满意的 spritesheet 做风格锚点，用用户照片做身份锚点。狗狗类宠物优先参考现有两只猫咪的细腻毛发、柔和阴影和半立体贴纸感，再叠加柯基/哈士奇/金毛照片里的品种特征。
4. 用 skill 内的提示词范本生成 8 列 x 9 行的透明 spritesheet，尺寸为 `1536 x 1872`，单格 `192 x 208`。
5. 使用 `scripts/pet_atlas.py package` 清理假透明背景、可选重排格子、生成 `pet.json`。使用 `--repack` 后必须人工检查 `spritesheet-grid-check.png`，避免重排漏格。
6. 用 `scripts/pet_atlas.py validate` 校验尺寸、透明度和元数据。
7. 如移动方向相反，用 `scripts/pet_atlas.py flip-rows --rows 2 3` 按单格翻转方向行；当前 Codex renderer 的实际契约是第 2 行向右、第 3 行向左。
8. 用 `scripts/pet_atlas.py install` 安装到 `~/.codex/pets`。

方向是每次必须检查的高频坑：当前 Codex renderer 中，第 2 行必须朝右、第 3 行必须朝左，这样实际移动时才会“向左移动向左跑、向右移动向右跑”。只修朝向时按单格 `flip-rows`，不要倒序整行。详细清单见 [Codex Pet 方向 QA 清单](docs/pet-direction-qa.md)。

脚本依赖 Pillow；如果系统 `python3` 没有 Pillow，可以在 Codex 里使用 bundled Python runtime。

更多提示词和排错细节见 `skills/codex-pet-maker/SKILL.md`。

## 新手教程

- [0 基础：跟着做一组 Codex 柯基宠物](docs/corgi-pet-zero-to-one.md)
- [0 基础：跟着做一组 Codex 哈士奇宠物](docs/husky-pet-zero-to-one.md)
- [0 基础：跟着做一组 Codex 金毛宠物](docs/golden-retriever-pet-zero-to-one.md)
- [0 基础：跟着做一组 Codex 酷儿宠物](docs/kuer-pet-zero-to-one.md)
- [0 基础：跟着做一组 Codex 兔子宠物](docs/rabbit-pet-zero-to-one.md)
- [Codex Pet 美术流程结论](docs/pet-artwork-pipeline-notes.md)
- [Codex Pet 方向 QA 清单](docs/pet-direction-qa.md)

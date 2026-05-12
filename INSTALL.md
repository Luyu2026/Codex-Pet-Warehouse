# Codex Pet Skill 安装说明

这份说明给 Codex / Claude Code / OpenCode 等 agent 使用。目标是把 `codex-pet-maker` skill 和内置宠物安装到本机 Codex。

## 1. 获取仓库

```bash
git clone https://github.com/Luyu2026/Codex-Pet-Skill.git
cd Codex-Pet-Skill
```

如果仓库已经存在，进入仓库后更新：

```bash
git pull origin main
```

## 2. 安装 Skill

```bash
mkdir -p ~/.codex/skills
cp -R skills/codex-pet-maker ~/.codex/skills/codex-pet-maker
```

## 3. 安装内置宠物

```bash
mkdir -p ~/.codex/pets
cp -R pets/* ~/.codex/pets/
```

如果只覆盖某一个宠物，使用复制目录内容的写法，避免出现同名嵌套目录：

```bash
mkdir -p ~/.codex/pets/<pet-id>
cp -R pets/<pet-id>/. ~/.codex/pets/<pet-id>/
```

## 4. 验证

```bash
python skills/codex-pet-maker/scripts/pet_atlas.py validate --pet-dir pets/lihua-cat-coder
```

如果系统 Python 没有 Pillow，在 Codex 里使用 bundled Python runtime 运行同一条命令。

## 5. 使用

重启 Codex 后，在对话里说：

```text
用 $codex-pet-maker 根据这张参考图做一个 Codex 宠物，并安装到自定义宠物文件夹。
```

生成完成后，打开 Codex 的自定义宠物列表选择对应宠物。若列表仍显示旧图，切换一次宠物或重启 Codex 清缓存。

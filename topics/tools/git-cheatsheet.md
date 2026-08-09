---
title: "Git 常用命令速查表"
summary: "日常开发中 90% 场景会用到的 Git 命令汇总"
document_created_at: "2026-08-09"
document_updated_at: "2026-08-09"
tags: ["git", "tool"]
related:
  - "../programming/python-best-practices.md"
confidence: "high"
---

# Git 常用命令速查表

> ⚡ 日常高频命令，够用到 90% 的场景了

---

## 基础 / Essentials

| 命令 | 说明 |
|---|---|
| `git clone <url>` | 克隆远程仓库到本地 |
| `git status` | 查看当前工作区/暂存区状态（最常用） |
| `git add <文件>` | 把文件加入暂存区 |
| `git add .` | 把当前目录所有修改加入暂存区 |
| `git commit -m "描述"` | 提交暂存区的内容 |
| `git commit -am "描述"` | 所有已追踪的文件的修改直接提交（跳过 add，不包含新文件） |
| `git log --oneline` | 查看简洁的提交历史（一行一个 commit） |
| `git diff` | 看工作区和暂存区的差异（改了什么还没 add） |
| `git diff --cached` | 看暂存区和 HEAD 的差异（add 了但还没 commit） |

---

## 分支 / Branches

| 命令 | 说明 |
|---|---|
| `git branch` | 查看本地分支 |
| `git branch -a` | 查看所有分支（含远程） |
| `git checkout <分支名>` | 切换分支 |
| `git switch <分支名>` | 切换分支（现代 Git 推荐） |
| `git checkout -b <新分支>` | 创建并切换到新分支 |
| `git switch -c <新分支>` | 同上，现代写法 |
| `git branch -d <分支名>` | 删除已合并的分支 |
| `git branch -D <分支名>` | 强制删除未合并的分支（谨慎） |
| `git merge <分支名>` | 把指定分支合并到当前分支 |
| `git rebase <分支名>` | 把当前分支"搬"到指定分支之上（线性历史） |

---

## 远程 / Remote

| 命令 | 说明 |
|---|---|
| `git remote -v` | 查看远程仓库地址 |
| `git fetch` | 拉取远程所有更新，但不合并到本地 |
| `git pull` | 拉取 + 合并远程当前分支（等同 fetch + merge） |
| `git pull --rebase` | 拉取 + rebase，避免多余 merge commit（推荐） |
| `git push` | 把本地提交推到远程 |
| `git push -u origin <分支名>` | 第一次推新分支，建立追踪关系 |
| `git push --force-with-lease` | 安全地强制推送（rebase 后要用），比 `--force` 安全 |

---

## 撤销 & 回滚 / Undo

| 场景 | 命令 |
|---|---|
| 撤销工作区某文件的修改（还没 add） | `git restore <文件>` 或 `git checkout -- <文件>` |
| 把某个文件从暂存区拿出来（add 了但不想 commit） | `git restore --staged <文件>` 或 `git reset HEAD <文件>` |
| 修改最后一次 commit（还没 push） | `git commit --amend -m "新描述"` |
| 回滚到上一个 commit（保留改动为未提交） | `git reset --soft HEAD~1` |
| 回滚到上一个 commit（彻底丢弃改动） | `git reset --hard HEAD~1` **⚠️ 危险** |
| 生成一个反向 commit 来撤销某次提交（已 push 到公共分支时用这个，不要 reset） | `git revert <commit-hash>` |

---

## 常见场景 / Recipes

### 场景 1：我改了一堆代码，现在想临时保存去处理别的事

```bash
# 存起来
git stash push -m "我正在写的功能描述"

# 处理别的事，切分支、commit 等...

# 回来，取出之前存的
git stash list                # 看存了哪些
git stash pop                 # 取最近一条并删除
# 或者 git stash apply stash@{0}  # 取用但不删除
```

### 场景 2：我在错误的分支上提交了，想挪到新分支

```bash
# 假设当前在 main，刚提交了不该在这的代码
git log --oneline            # 记下要挪的 commit hash，假设是 abc123
git reset --hard HEAD~1      # 先把 main 撤回去（如果还没 push）
git switch -c feature-branch
git cherry-pick abc123       # 把这个 commit 搬过来
```

### 场景 3：解决 merge 冲突

```bash
# pull 或 merge 后提示冲突
git status                    # 看哪些文件是 both modified
# 打开文件，找到 <<<<<<< ======= >>>>>>> 标记，手动改成想要的样子
git add <解决完的文件>
git commit                    # 完成 merge
```

### 场景 4：查看某次 commit 改了什么

```bash
git show <commit-hash>        # 看某次 commit 的详情和 diff
git show <commit-hash>:<文件路径>  # 看那个版本的某文件内容
```

---

## ⚠️ 安全红线

- **不要在公共分支（main / master / develop）上 `rebase` 或 `reset --hard`**
- `git reset --hard` 用前一定要确认，工作区未提交的修改会直接消失
- `git push --force` 只在你确定没人在这个分支上工作时用，尽量用 `--force-with-lease`

# 学习笔记目录

这里放我学习 DeepSeek Harness 的个人文档。**它不是仓库内容**，不参与构建、不参与文档门禁、不提交。

## 为什么单独开一个目录

- `docs/` 是仓库的产品文档，受链接校验、字数预算、中英配对等门禁约束，个人笔记放进去会被当成项目文档维护。
- 学习笔记会频繁改、会写错、会过期，放在一个独立目录里可以随便改。
- 将来笔记多了，这里就是一个索引，不用在仓库根目录堆一堆 Markdown。

## 约定

- **链接**：目录内文档指向仓库文件的相对路径一律以仓库根为基准，写 `../docs/...`、`../packages/...`。这样在编辑器里点击可跳转，文件搬动也不影响仓库本身的链接。
- **不提交**：当前目录没有 `.git`，所以它只是未跟踪文件。将来若仓库带 `.git`，执行一次下面的命令即可彻底隐藏（只在本机生效）：

  ```sh
  echo 'learning/' >> .git/info/exclude
  ```

- **新增文档**：放在本目录下，并在下面的表格里登记一行。

## 文档索引

| 文档 | 内容 | 更新 |
|---|---|---|
| [LEARNING.md](LEARNING.md) | 七阶段学习路线清单，含通关标准与概念对照 | 2026-10-28 |

## 计划中

- [ ] `faq.md` —— 命令答疑：`dev:web` vs `dsh web`、`pnpm exec` vs `pnpm run`、单测路径过滤、TS 版本切换
- [ ] `glossary-notes.md` —— 我自己的术语对照表（plugin / service / inject / effect / seam / turn / step）
- [ ] `environment.md` —— 本机环境与 IDE 配置（tsdk、watcherExclude、tasks.json）
- [ ] `reading-notes/` —— 每个阶段的读码笔记，一包一篇

## 当前进度

见 [LEARNING.md 的「进度记录」](LEARNING.md#进度记录)。

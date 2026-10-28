# DeepSeek Harness 学习路线（个人清单）

> 这份清单属于 `learning/` 目录（见 [README](README.md)），**不是仓库内容、不提交**。
> 用法：按阶段从上往下走，勾掉 `- [ ]`；每阶段末尾有「通关标准」，答不上来就别进下一阶段。
> 链接一律以仓库根为基准（`../` 开头），在编辑器里可以直接点击跳转。

## 总览

| 阶段 | 主题 | 业余 2h/天 | 通关凭证 | 状态 |
|---|---|---|---|---|
| 0 | 跑起来 | 半天 | typecheck 绿 + 单测绿 + 界面打开 | ✅ 2026-10-28 |
| 1 | TypeScript 最小必要集 | 3–4 天 | 改写一个真实包并补测试 | 进行中 |
| 2 | Cordis 插件模型 | 1 周 | 教程 7 章通关 + 插件挂进 Web | |
| 3 | turn / session 主干 | 1–2 周 | 手画出一次对话的事件流 | |
| 4 | 工具纵切 | 1 周 | 自写工具在 UI 有专属卡片 | |
| 5 | 选方向深入 | 2–4 周 | 能定位 issue 属于哪个包 | |
| 6 | 参与贡献 | 持续 | 一个被合入的小 PR | |

核心原则：**这个仓库有 369 个包、466 个 tsconfig，永远纵切，永不横读。**

---

## 阶段 0 · 环境基线（半天）✅

- [x] 依赖已安装（`node_modules/.pnpm/typescript@6.0.3` 存在）
- [x] 仓库已有 `lib/types` 构建产物
- [x] `node -v` ≥ 24.9，`pnpm -v` = 11.28.5
- [x] `pnpm run typecheck` 通过 ← 官方的「环境就绪」信号，见 [docs/development.zh.md](../docs/development.zh.md)
- [x] `pnpm exec vitest run packages/util/code-language` 通过
- [x] `pnpm run dev:web` 能打开 Web 界面（无 API key 也能看界面）
- [x] VS Code 已切到 workspace TypeScript 6.0.3（中文界面：`Ctrl+Shift+P` → 「TypeScript: 选择 TypeScript 版本」，**必须先打开一个 `.ts` 文件**这条命令才出现）
- [x] `.vscode/settings.json` 写好 `typescript.tsdk`

**命令速查**

```sh
pnpm run typecheck                                  # 全仓类型检查，几分钟
pnpm exec vitest run packages/util/code-language    # 单包测试，秒级
pnpm exec vitest run <某个 .spec.ts 路径>            # 单文件测试
pnpm run dev:web                                    # 构建 + 起服务 + 监听重建
pnpm dsh web                                        # 只起服务（用已有产物）
pnpm dsh --profile headless "任务"                   # 无界面跑一次任务（需 API key）
```

**不要做**：`pnpm test`（会先编原生插件，很重）、`pnpm run build`（全量构建）。

---

## 阶段 1 · TypeScript 最小必要集（3–4 天）· 进行中

只学这个仓库真正用到的，别啃完整本教程。

- [ ] 类型标注、`interface` vs `type`、泛型、联合类型、字面量类型
- [ ] 类型收窄（`typeof` / `in` / 判别字段 `kind` 的 switch）
- [ ] `readonly` / `as const` / 不可变表
- [ ] `import type`（编译后消失，不产生运行时依赖）
- [ ] **声明合并**（`declare module '@deepseek-ai/cordis'`）← 最重要，Cordis 的 service 和 event 全靠它
- [ ] 本仓库特异点：相对导入带 `.ts` 后缀；`exactOptionalPropertyTypes` 下 `?` 与 `| undefined` 不等价；`noUncheckedIndexedAccess` 下数组下标是 `T | undefined`
- [ ] 读 [packages/util/code-language/src/index.ts](../packages/util/code-language/src/index.ts)（233 行，纯函数、零依赖）
- [ ] 读 [packages/util/code-language/tests/code-language.spec.ts](../packages/util/code-language/tests/code-language.spec.ts)（表驱动测试样板）
- [ ] 练习：给它加一个新后缀（如 `zig`），两张表各加一行 + 测试加一行，跑绿

**教材**（只看这三页，其余现查）
- <https://www.typescriptlang.org/docs/handbook/2/everyday-types.html>
- <https://www.typescriptlang.org/docs/handbook/2/narrowing.html>
- <https://www.typescriptlang.org/docs/handbook/declaration-merging.html>

**通关标准**
- [ ] 能解释 `LANGUAGES` 为什么用 `Map` 而不是对象（原型链污染：`foo.constructor`）
- [ ] 能自己写 `it.each` 表驱动测试
- [ ] 改完 `pnpm run typecheck` 绿

---

## 阶段 2 · Cordis：这个项目的心脏（1 周）

这是从 Java/Python/C++ 转过来**唯一的真概念断层**：没有 `main()`，没有 Spring 反射扫描，整个产品是一棵插件树。

- [ ] 读 [docs/cordis-primer.zh.md](../docs/cordis-primer.zh.md)（45 行，20 分钟）
- [ ] 教程第 1 章 [第一个插件](../docs/cordis-tutorial/01-first-plugin.zh.md)
- [ ] 教程第 2 章 [生命周期与副作用](../docs/cordis-tutorial/02-lifecycle-and-effects.zh.md)
- [ ] 教程第 3 章 [服务](../docs/cordis-tutorial/03-services.zh.md)
- [ ] 教程第 4 章 [事件](../docs/cordis-tutorial/04-events.zh.md)
- [ ] 教程第 5 章 [配置](../docs/cordis-tutorial/05-config.zh.md)
- [ ] 教程第 6 章 [组合与 HMR](../docs/cordis-tutorial/06-composition-and-hmr.zh.md)
- [ ] 教程第 7 章 [进入 harness](../docs/cordis-tutorial/07-into-the-harness.zh.md)（注册真实工具）
- [ ] [你的第一个 Harness 插件](../docs/user/develop/basic/index.zh.md)，挂进 Web UI：`pnpm dsh web --patch ./scratch-plugin/cordis.yml`

教程在 scratch 目录里跑（`tmp/` 已被 gitignore）：
```sh
mkdir -p tmp/cordis-tutorial && cd tmp/cordis-tutorial
node --import tsx ../../vendor/cordis/bin.js
```

**通关标准（自测题）**
- [ ] plugin / service / inject / effect / event / waterfall 六件事各用一句话说清
- [ ] 为什么注册必须返回 disposer？不返回会怎样？
- [ ] waterfall 里忘了调 `next()` 会发生什么？
- [ ] 服务为什么按 `ctx.tools` 这样的 key 取，而不是 `import` 具体实现？
- [ ] 可选服务为什么用 `ctx.get(name)` 而不是 `ctx.<name>`？

---

## 阶段 3 · 主干：一次 turn 怎么跑完（1–2 周）

每步**先读 README，再读 `src/index.ts`，再看 `tests/`**。

- [ ] [docs/architecture.zh.md](../docs/architecture.zh.md)（169 行，1 小时，配合 turn flow 图）
- [ ] [docs/agent-lifecycle.zh.md](../docs/agent-lifecycle.zh.md)
- [ ] [docs/tool-execution-pipeline.zh.md](../docs/tool-execution-pipeline.zh.md)
- [ ] 读 `packages/core/session`（事件日志，先 README）
- [ ] 读 `packages/core/tools`（工具注册表与执行管线）
- [ ] 读 `packages/core/agent`（Agent 接口与注册表）
- [ ] 读 `packages/core/agent-loop`（默认驱动，最后读）
- [ ] 实验：跑一次真实对话（需 `DEEPSEEK_API_KEY`），去 `~/.dsh`（或 `$DSH_HOME`）下的 sessions 目录翻 JSONL，把事件和 UI 上看到的东西逐条对上

**两个必须建立起来的认知**
- [ ] **持久事件 vs 实时事件**：`turn/*`、`step/*`、`user/message`、`assistant/message`、`tool/*` 是可回放的 session 事件；`agent/assistant-stream` 是进程内瞬时的。分界线是「模型能看到的东西必须能从日志重建」。
- [ ] **turn 与 step**：step = 一次模型请求 + 它调用的工具；turn = 零或多个 step，从领到输入到没有欠账。

**通关标准**
- [ ] 合上文档手画：`turn/start → claim → step/start → agent/request → llm/stream → tool/call → tool/result → step/end → turn/end`，再对照架构文档修正
- [ ] 能说出「model-visible ⟺ logged」在代码里是怎么保证的

---

## 阶段 4 · 一条完整纵切：工具从注册到 UI（1 周）

- [ ] 读 [docs/user/develop/basic/tool.zh.md](../docs/user/develop/basic/tool.zh.md)，写一个最小工具（建议 `word_count`：吃路径返回词数）
- [ ] 读 [docs/cookbook/adding-a-tool.zh.md](../docs/cookbook/adding-a-tool.zh.md)，理解 `defineTool` 契约：`parameters` 自动校验、`output.schema` 返回规范 JSON、`output.render` 给模型看
- [ ] 读生产级范例 [packages/shell/tool-bash/src/index.ts](../packages/shell/tool-bash/src/index.ts)（三件套：`index` / `background` / `render`）
- [ ] 分清两套输出：`output.render`（给模型）vs `presentCall` / `presentResult`（给人）
- [ ] 没有 API key 也能验证：用 `ctx.tools.execute()` 写单测走完管线
- [ ] 有 key 就让它出现在 Web UI 里，并给它做专属卡片（不要停在 generic 卡片）

**通关标准**
- [ ] 工具在 UI 显示专属卡片而非原始 JSON
- [ ] 有单测覆盖成功与失败路径
- [ ] `pnpm run typecheck` 绿

---

## 阶段 5 · 选一个方向深入（2–4 周，四选一）

- [ ] 方向 A · Agent / LLM 内核 → [docs/subsystems/README.zh.md](../docs/subsystems/README.zh.md)、[LLM 流式](../docs/subsystems/llm-streaming.zh.md)；包：`core/*`、`llm/*`、`compaction/*`
- [ ] 方向 B · Web 前端 → [Web 客户端](../docs/subsystems/web-client.zh.md)、[API 网关](../docs/api-gateway.zh.md)；包：`client/*`、`api/*`
- [ ] 方向 C · 执行层（沙箱 / 子进程 / 终端 / SSH）→ [capability-seams](../docs/capability-seams.md)；包：`sandbox/*`、`subprocess/*`、`terminal/*`（**C++ 背景最省力的一条**）
- [ ] 方向 D · 工程化与 CI → [docs/development.zh.md](../docs/development.zh.md)、[docs/testing.zh.md](../docs/testing.zh.md)；目录：`scripts/`、`packages/test-support/*`

**通关标准**
- [ ] 能对一个真实 issue 说出「这属于哪个包、改哪个扩展点、要补哪层测试」

---

## 阶段 6 · 参与贡献（持续）

- [ ] 读 [docs/defensive-patterns/README.md](../docs/defensive-patterns/README.md)
- [ ] 读 [docs/testing.zh.md](../docs/testing.zh.md)
- [ ] 读根 [AGENTS.md](../AGENTS.md) 的 Conventions 段
- [ ] 从最小的事开始：修一处文档 / 补一个边界测试 / 加一个设置卡片
- [ ] **先别碰 `agent-loop`**：改它要同步更新 `docs/architecture.md` 和两个 SDK 的期望输出

**PR 前自查**
- [ ] `pnpm run typecheck`
- [ ] 相关包的单测
- [ ] 改了对外可见行为 → 需要快照；改了文档 → `pnpm run test:docs`

---

## 概念映射（Java / Python / C++ → 这里）

| 你熟悉的 | 这里对应的 |
|---|---|
| Spring Bean / `@Autowired` / `ApplicationContext` | Service / `inject` / `ctx` |
| `@EventListener` | `ctx.on(...)`；waterfall 是可环绕的中间件链，不是广播 |
| `try-with-resources` / `@PreDestroy` | `ctx.effect(() => disposer)`，插件卸载自动回滚 |
| Maven 多模块 / `pom.xml` | pnpm workspace + `package.json` + tsconfig project references |
| `javac` / 增量编译 | `tsc -b`；IDE 里的 tsserver 是它的常驻版 |
| JUnit 参数化测试 | Vitest 的 `it.each` |
| Java 名义类型（必须 `implements`） | **结构化类型**：形状对就兼容，不用声明实现（最大的直觉错位） |
| Python 鸭子类型 / C++ 模板 | 更像 TS 的泛型 + 结构类型，但类型在运行时全部擦除 |
| Lombok / 注解处理器 | 没有；这里的「元编程」只有声明合并 + 代码生成脚本 `scripts/gen-*.ts` |

---

## 六个必踩的坑

1. **把 369 个包当书读** —— 读到第 10 个就放弃。永远纵切。
2. **以为类型能保护运行时** —— 模型输出、JSON、配置、子进程边界都要显式校验。
3. **读 `lib/` 里的 `.d.ts`** —— 那是构建产物，源码在 `src/`。
4. **IDE 全局重命名后不跑 `typecheck`** —— 跨包、跨 host/client 两个编译器面，IDE 看不全。
5. **在 `agent-loop` 里加业务逻辑** —— 仓库硬规矩是「插件，不是改循环」。
6. **跑全量 `pnpm test` 验证一个小改动** —— 单包 `vitest run <path>` 就够。

## 卡住时的查找顺序

1. 术语不懂 → [docs/glossary.zh.md](../docs/glossary.zh.md)
2. 这个事件谁发谁收 → [docs/event-producer-consumer.md](../docs/event-producer-consumer.md)
3. 类型定义 → [docs/subsystems/](../docs/subsystems/README.md) 对应页面
4. 这个包干什么 → 包自己的 `README.md`
5. 想加新行为 → [docs/architecture.zh.md](../docs/architecture.zh.md) 的「Where new behavior goes」表
6. 还是不懂 → 搜测试文件，测试就是行为规格

---

## 本周三件事

- [x] `pnpm run typecheck` 通过
- [ ] 读 `code-language` 的 233 行，加一个后缀 + 测试，跑绿
- [ ] 做完 Cordis 教程第 1 章（20 分钟）

## 进度记录

| 阶段 | 开始 | 完成 | 产出 / 笔记 |
|---|---|---|---|
| 0 | 2026-10-28 | 2026-10-28 | typecheck + 单包测试 + Web 界面 + TS 6.0.3 全部就绪 |
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 | | | |

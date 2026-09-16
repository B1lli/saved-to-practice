# 收藏变行动 · Saved to Practice

把收藏中的方法变成下次做事时用得上的技能。

这是一套 Agent skill 和本地工具：读取获准的收藏原文，判断它是否能改善用户的核心痛点，优先补强已有技能；通过效果与自然触发验证后再启用。不是每篇收藏生成一个技能。

## 当前范围

- 可安装的中文 skill，适用于能读文件、执行工具和发现技能的 Agent。
- 标准库实现的本地 JSON 导入、去重、内容更新及待评队列；无网络请求，不改原收藏。
- 价值筛选、候选隔离、效果对照、关键步骤消融、自然触发及回退流程。
- 日程启动模板；需要宿主的调度工具和用户的一次授权。
- 小红书/微信是可接入来源：优先使用宿主内置浏览器，没有则使用可选的 Playwright 回退，**本项目不内置免登录采集器，也不保证所有宿主能读到收藏**。

验证结果和限制见 [消融结果](evals/RESULTS.md)、[独立运行审核](docs/runtime-review.md) 和 [评测说明](docs/evaluation.md)。本地工具测试和编排决策测试不证明“用户只点收藏后，真实工作自动改善”。

采集回退的安装、登录和读取方法见 [Playwright 指南](skills/saved-to-practice/references/playwright.md)。附带通用页面采集脚本；列表分页和图片完整性仍需 Agent 检查，不是免登录的站点爬虫。

## 本机实跑

已在登录小红书的本机完成一次真实收藏 → 评估 → 保存 → 去重：打开 2 篇笔记，2/2 判断读回成功；重复导入 0 新增、2 不变。完整阅读与部分阅读分别记录，没有将未验证的方法激活。见[实跑范围与限制](docs/local-run.md)。

## 安装

需要 Python 3.10+；本地工具无第三方依赖。

```sh
git clone https://github.com/B1lli/saved-to-practice.git
cd saved-to-practice
python3 scripts/install.py
```

默认安装到 `$CODEX_HOME/skills/saved-to-practice`，未设置时用 `~/.codex/skills/saved-to-practice`。其他宿主可指定其支持的目录，例如：

```sh
python3 scripts/install.py --skills-dir ~/.claude/skills
```

已有版本默认不覆盖；`--replace` 会备份旧版本后替换。启动新会话确认技能可见，再说：

> 用 saved-to-practice 把我收藏里的有效方法接入日常工作，先检查来源和我的实际痛点。

普通工作由后续验收的专项技能承担，不需要每次点名入口。安装本项目不会自动开日程，也不会自动读取微信。

## 先用一份导出跑起来

把获准的原文整理成 [示例格式](examples/notes.json)：每条包含 `source`、稳定 `id`、`title`、`url`、`content`、布尔 `complete`。示例文本为本项目原创合成数据。

```sh
python3 skills/saved-to-practice/scripts/inbox.py --data-dir .local/demo ingest examples/notes.json
python3 skills/saved-to-practice/scripts/inbox.py --data-dir .local/demo pending
```

Agent 读待评原文，结合用户的真实任务和失败证据判断。用当前 `revision` 记录判断：

```sh
python3 skills/saved-to-practice/scripts/inbox.py --data-dir .local/demo assess reading-export example-01 --revision 1 --decision needs-evidence --reason '尚无真实失败产物，先保留方法候选'
```

`skip`、`candidate`、`needs-evidence` 是 Agent 的当前判断记录；**工具不会把 candidate 自动安装或认定有效**。完全相同的重复导入不重复待评；标题、链接、正文或完整性标记变化会再次待评。`pending` 只读，不会提前消费条目。新失败证据出现时，Agent 可重新评估已存条目（按稳定 ID 查询，见技能工具说明）。

本地数据默认在 `~/.local/share/saved-to-practice`；可用 `--data-dir` 或 `SAVED_TO_PRACTICE_DATA` 改位置。与代码仓库分离保存个人原文、用户画像、来源授权和评测私料。

## 日程与停止

首次启动约定来源、数量、时间和时区，再授权宿主创建每日任务。当前支持固定时间方案；是否能在关机/睡眠时执行以及补跑由宿主决定。“当天最后一次开机”不作为已实现能力。

停用方法：移出对应技能，或恢复上个版本。停用采集：暂停宿主中的日程。二者互不隐含，均不删除原收藏。

## 验证与贡献

```sh
python3 -m unittest discover -s tests -v
```

模型消融另行运行，依赖可用的 Agent CLI 登录；详见 [评测说明](docs/evaluation.md)。提交修复时保留失败输出，区分工具故障、决策错误和真实用户效果，不用重复重试挑选绿结果。

## License

[Apache License 2.0](LICENSE)，Copyright 2026 B1lli。收藏原文、第三方服务和导出数据不因本项目而更改其权利或条款。

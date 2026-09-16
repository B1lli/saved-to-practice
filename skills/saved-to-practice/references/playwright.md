# Playwright 回退

宿主没有可用的内置浏览器工具时使用。先发现当前实际工具；不能仅因产品叫 Codex/Claude 就认定有浏览器，也不要求 ego lite、Edge 或 Chrome。

## 一次安装

复用可用的 Playwright 工具/运行环境；缺少时在私人数据目录建虚拟环境，安装官方 Playwright 和它管理的 Chromium。macOS/Linux 示例：

```sh
python3 -m venv ~/.local/share/saved-to-practice/playwright-venv
~/.local/share/saved-to-practice/playwright-venv/bin/python -m pip install playwright
~/.local/share/saved-to-practice/playwright-venv/bin/python -m playwright install chromium
```

Windows 可用 `python -m venv <数据目录>/playwright-venv`，解释器为 `<数据目录>/playwright-venv/Scripts/python.exe`。Linux 若缺系统库，按 Playwright 官方安装说明补充系统依赖；不能将启动失败记为空收藏。

## 登录与读取

用上面的虚拟环境解释器运行本技能的 `scripts/browser.py`：

```sh
<python> <skill-dir>/scripts/browser.py '<用户的收藏页URL>' --login
<python> <skill-dir>/scripts/browser.py '<已观察到的帖子URL>'
```

`--login` 必须通过交互终端/PTY运行，向用户展示窗口，让用户完成登录后在终端按回车。登录态仅保存在数据目录的 `browser-profile/`；不要复制日常浏览器的凭证，也不要提交或分享这个目录。相同 profile 同时只运行一个实例。登录态与宿主内置浏览器不共享，首次回退可能需要重新扫码。

每次输出私人 `capture_dir`，包含 `page.json` 的正文、链接、图片地址以及 `page.png` 全页截图。动态页面可用 `--wait-for '<实际观察到的选择器>'` 等待明确内容；不要编造选择器。页面的 `complete` 一律为 false、`source_status` 为 unverified，**退出码0仅证明采集完成**。

Agent 检查截图与正文：确认账号、收藏标签、登录/验证码/错误页以及列表加载是否完成。不能把登录页中的空链接当零收藏。按照 sources.md 限制观察范围，只跟随实际读取的帖子链接。需要翻页、滚动、切换图片时，使用 Playwright 工具；没有该工具但能执行脚本时，依据当前页面证据用 Python Playwright API 操作同一个专用 profile，再读取新状态。辅助脚本只是通用页面采集，不是完整小红书站点适配器。

图片轮播不一定出现在全页截图中，视频也不是正文。读完关键图片/可核实视频内容后，才能由 Agent 将原文整理成 inbox.py 的导入格式并判断 complete。原文、带访问参数的 URL、截图和浏览器状态都只留本机私人目录。

登录失效或平台限制时通知用户必要动作，停止该来源；不绕过验证码，不自动修改收藏。Playwright 提供网页访问能力，不代表存在微信收藏网页入口。

官方参考：[安装](https://playwright.dev/python/docs/intro)、[专用持久化上下文](https://playwright.dev/python/docs/api/class-browsertype#browser-type-launch-persistent-context)。

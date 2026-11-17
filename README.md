[README.md](https://github.com/user-attachments/files/34971689/README.md)
# 像素定格 v0.2.0

**当前版本请先阅读 [v0.2.0 运行说明](docs/v0.2-running.md)。** 新包为 `deliverables/像素定格-H5-v0.2.0.zip`，Windows启动不需Node.js，含调色挑战第4、5关。配套五关视频与5页PDF已更新。下方v0.1.0内容为历史三关版本说明。

滑动颜料，补全像素图案；填好的像素会凝固成障碍，改变后续路线。

## 开始游戏

正式包位于 `deliverables/像素定格-H5-v0.1.0.zip`。解压后双击“开始游戏.cmd”；需要 Node.js 22.12 或更高版本，不需要安装 npm 依赖。其他系统执行 `node serve.mjs`，打开 `http://localhost:8080`。

也可把包内 `game/` 全部部署到静态服务器的根目录或子目录，通过 HTTP 访问。不能双击 HTML 代替 HTTP 启动。手机与电脑处于同一局域网时，打开启动终端显示的局域网地址；系统若弹出网络访问提示，按自己网络环境处理。

开发与验证：

```powershell
npm ci
npm run dev
npm run check
npm run test:e2e
```

浏览器验收使用已安装的 Microsoft Edge，`npm run test:e2e` 直接验证 `dist/` 正式包。Node 当前验证版本为 24.20.0，运行时没有联网服务、AI 调用、远程字体或图片。

## 操作

- 棋盘滑动、方向按钮、方向键或 WASD：所有颜料向同一方向滑动。
- 圆形珊瑚色和菱形薄荷色各自匹配同形目标；经过首个匹配目标即被吸收。
- 空心目标可穿过，凝固像素和墙不可穿过。凝固立即生效。
- 撤销不限次数；重开恢复当前关；动画期间方向输入丢弃，撤销和重开立即生效。
- 顶部可直接选择三关；音效默认关闭，可手动开启。

## 交付文件

- `deliverables/像素定格-H5-v0.1.0.zip`：独立 H5 包及本地启动器。
- `deliverables/像素定格-三关演示.mp4`：720×1280 竖屏、真实按钮操作，含三关完整通关，静音。
- `deliverables/像素定格-三关完整流程.webm`：原始浏览器录制。
- `deliverables/像素定格-关卡设计.pdf`：4 页，含概念图、核心循环和三关设计意图。
- `deliverables/manifest.json`：冻结版本、构建指纹、文件 SHA256。
- `docs/player-design.html`、`docs/player-design.md`：可编辑设计材料。
- `verification/levels.json`：搜索预算、通关序列、重放、凝固支点及错误分支证据。
- `verification/browser-results.json`、`verification/capture.json`：浏览器验收和录制记录。

## 三关及设计

| 关卡 | 规模 | 最短有效步 | 核心思考 |
|---|---|---|---|
| 第一枚支点 | 4×4，2 目标 | ↓ ← ↓ | 凝固后成为下一枚颜料的落点 |
| 借一个转角 | 5×5，3 目标 | → ↑ ← ↓ → ↑ | 区分异色穿过与凝固阻挡 |
| 连锁定格 | 5×5，4 目标 | → ↓ ↑ ← ↓ ← ↓ ← ↓ | 最后一个目标依赖前面的多个支点 |

解由单位代价 BFS 按层找到，并通过生产规则重放。独立测试另有手算预期，不把共享规则视为正确性的唯一证据。错误分支完整无解指具体局面，不泛化成所有同目标开头的路线。

## 复现交付

```powershell
npm run check
npx tsx tools/design-document.ts
npm run test:e2e
# 单独终端：npm run preview -- --port 4173 --strictPort
npx playwright install ffmpeg
npx tsx tools/capture.ts
npm run deliver
```

生成 PDF、录像需要本机 Edge；MP4 转码使用锁定的 Windows ffmpeg 工具，仅为制作依赖。录像、PDF 完成后若改动关卡或构建，必须重新生成再打包；`manifest.json` 与 `verification/capture.json` 记录对应构建指纹。

## 验证边界及提交前事项

已完成规则测试、三关机器求解及正式包浏览器通关、触摸模拟、连续输入与动画取消、相对资源路径检查。尚未进行实体 Android/iPhone 测试、真人盲测或官方提交。机器最短解不代表真人难度或核心乐趣已被证实。

按 2026 年 12 月 6 日安排截止；计划 12 月 3 日完成提交预检，12 月 4–5 日缓冲。官方截止时区/时刻、H5 接收网址还是 ZIP、视频格式限制、署名模板和资源披露要求仍待确认。比赛条款来自用户输入，未声称已核验官方公告。

不声明玩法全球原创。第三方引擎许可见 `THIRD_PARTY_NOTICES.txt`。图形和音效由代码生成，无外部美术或音频素材。

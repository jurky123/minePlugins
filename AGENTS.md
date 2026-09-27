# minePlugins 开发约定

本目录是服务器插件的汇总仓库（git submodule），每个子目录是独立仓库、独立发布。
以下约定对 mineUI / mineUNO / mineChess / mineSkin / mineAgent 以及后续开发的所有插件/mod 全部适用。

## 动手前

- 先读目标项目的 README 和现有代码以及设计文档，遵循既有的包结构、命名和写法，不要另起一套。
- 确认需求的功能是否可以使用现有的插件API，如果没有现成功能确认是否在现有的API插件职责内，如果都没有确认是一个新的类别API需求还是只是本插件独有实现即可。
- 跨项目改动先确认影响谁：MineChess/MineSkin 依赖 mineUI 业务 API，部署顺序和版本要对齐。如果有跨项目改动需求不要擅自动手，提需求即可。

## 代码风格与设计原则

- 简单直接优先：能在一两个类里讲清楚的逻辑，不要拆成多层。
- 不过度耦合：模块/插件之间只通过公开 API、事件或简单数据交互；不直接访问对方内部状态。
  接入 MineUI 等提供API服务的插件 一律走业务 API例如 `com.mineui.api`，客户端不支持时按既有模式回退原版实现。
- 遵循软件设计原则（单一职责、高内聚低耦合、组合优于继承、依赖最小化），但以可读性为准，
  不要为想象中的未来需求提前抽象。
- 不要过度模块化：不建只有一个实现的接口、一次性工具类、过早泛化的"框架"；
  出现第二个真实用例时再抽取。
- 匹配现有代码风格（命名、结构、注释与中文文案习惯）；不随意引入新依赖或框架，确需引入时说明理由。
- 改动小而聚焦，不做与需求无关的重构；API/行为/配置变化同步更新对应 README 或 docs。
- 及时提交推送改动，改动变化写清楚，及时更新相关README或docs，以及上层仓库的文档。

## 项目索引

### mineUI — 自定义 UI 框架（Paper 插件 + Fabric mod）

- 文档：`mineUI/README.md`、`mineUI/docs/DESIGN.md`（协议/技术设计）、`mineUI/docs/COMPONENTS.md`（组件与素材）
- 模块：`mineui-protocol`（协议）/ `mineui-paper`（插件）/ `mineui-client`（Fabric mod）
- 业务 API：`com.mineui.api`（`MineUi` / `MineUiProvider` / `MineUiSession` / `MineUiAction`）
- 构建：`./gradlew :mineui-paper:build`；给其它插件编译用：`./gradlew :mineui-paper:publishToMavenLocal`；
  客户端安装包：`tools/build_client_kit.sh`

### mineUNO — 3D UNO 卡牌 + PackHost

- 文档：`mineUNO/README.md`（玩法/命令/配置/材质包）
- 源码：`mineuno/src/main/java/com/mineuno/`（`MineUnoPlugin` / `MatchManager` / `Game` / `Table` / `Menus` …）；
  材质包托管：`packhost/src/main/java/com/mineuno/packhost/`
- 构建：`./build.sh`（产物在 `out/`）；材质包生成：`pack/gen_pack.py`

### mineChess — 3D 国际象棋

- 文档：`mineChess/README.md`（目录结构、配置、资源包说明都在里面）
- 关键位置：`com.minechess.MatchManager`（总控）、`match/`（规则/房间/棋钟）、`view/`（3D 棋盘）、
  `arena/`（坐标变换）、`ai/ChessBot`（可插拔 AI）、`integration/MineUiControl`（MineUI 页面）；
  页面 JSON：`src/main/resources/assets/minechess/ui/chess/*.json`
- 构建/部署：`./build.sh`、`./deploy.sh`；测试：`mvn -B test`；依赖 mineUI 前先 `publishToMavenLocal`

### mineSkin — 服务器皮肤入口

- 文档：`mineSkin/README.md`
- 源码：`com.mineskin`（`MineSkinPlugin` / `SkinCatalog` / `SkinBrowser` / `ChatSkinList`），
  `ui/` + `integration/` 是与 MineUI 解耦的适配层；页面 JSON：`src/main/resources/assets/mineskin/ui/skin/browser.json`
- 构建/部署：`./deploy.sh`（会先构建 mineUI）

### mineAgent — AI 聊天助手

- 文档：`mineAgent/README.md`
- 结构：Go 后端 `internal/`（agent / tools / session / storage / ws / qq / wecom / aibot / wechat / **account** / **portal** / webui …）+ `cmd/mineagent`；
  **Mine 门户**（`internal/portal`，:8766）：`/` 门户首页（公告栏 + MC 状态卡 + 应用卡片）、`/agent` 聊天、`/account` 账号、
  `/games` 小游戏目录（国际象棋 `/games/chess`、五子棋 `/games/gomoku`，均为**在线房间对战**；
  `internal/games` 通用棋室（Match 接口 + 注册表 + 内存房间 + SSE，API `/api/games/<id>/*`），
  规则在各游戏包（chess/ 不含吃过路兵与和棋规则；gomoku/ 15 路黑先连五，不做禁手）；
  一局结束写 `game_runs`；前端大厅/房间共用 `static/js/gameroom.js`）；
  账号在 `internal/account`（users/auth_sessions，名字即账号无密码，token 只存 sha256）；会话键 `web:user:<用户ID>`，
  上传目录 `web-files/u<ID>`；新增门户功能 = 在 `cmd/mineagent/portal_apps.go` 注册一个 `portal.App`（不改骨架）；
  设计文档 `MINE_PORTAL_DESIGN.md`（账号/迁移/游戏平台规划与实施记录）；前端为 static/ 下的原生 ES 模块 + 分层 CSS，
  交互原语在 `css/ds.css` + `js/ds.js`（Button/Dialog/Confirm/Toast/Field…，页面禁止 alert/prompt/confirm），
  首页按 App 的 `HomeRole`（hero/status/content/feed）分区渲染，导航只放 `Nav=true` 的应用；无构建步骤；
  Paper 插件 `paper-plugin/src/main/java/com/mineagent/paper/`
- 构建：`make build && make plugin`；安装：`scripts/build.sh --install`；运行：`systemctl restart mineagent`
- 数据迁移（改键/换结构时）：`./bin/mineagent --config config.json --migrate-portal`（先停服，自动 VACUUM INTO 备份、幂等可重跑）
- **域名**：`https://zkun.art/`（内置 autocert 自动签发/续期 Let's Encrypt 证书，配置项
  `web.domain`；证书缓存在 `/home/ubuntu/mineagent-data/webui/certs`）与 `http://zkun.art/`
  （80 端口与企微回调共存：`/wecom` 走回调，其余路径由 `wecom.Channel.WithFallback(...)` 转发，
  80 同时服务 ACME 验证）。放行：控制台 TCP 80/443（0.0.0.0/0 与 ::/0；DNS 只有 A 记录时 IPv4 必须放行）。
- **部署（必须先查对局）**：用 `scripts/deploy.sh`（构建 → 查 `/api/games/active` 有没有进行中的对局
  → 有人在下棋就拒绝重启，`--force` 强制；管理员令牌读 `data/webui/deploy-token.txt`，0600 不进仓库）。
  **重启会清空内存里的棋局房间**，所以不要直接 `systemctl restart`。

### mineAudio — 统一音频基础设施（音乐/环境音/游戏音效）

- 文档：`mineAudio/README.md`（命令/配置/素材规范）、`mineAudio/docs/PLAN.md`（落地方案与实施记录）、`mineAudio/初步设计文档`（架构设计）
- 源码：`mineaudio-api`（`com.mineaudio.api` 业务 API）/ `mineaudio-paper`（`com.mineaudio`：playback / backend / region / emitter / track）
- 构建：`./gradlew build`（含单测）；API 发布：`./gradlew :mineaudio-api:publishToMavenLocal`；部署：`./deploy.sh`（插件 + 资源包交给 PackHost + NBS）
- 说明：V1 已完成；MineUNO/MineChess 接入、PackHost 独立化、NoteBlockAPI 安装为后续事项

### 后续其他插件加入时更新AGENTS.md

## 提交与部署

- 每个子目录是独立 git 仓库：修改、提交、推送都在对应仓库进行，不要动汇总仓库的子模块指针（除非用户要求同步版本）。
- 不提交构建产物（`build/`、`target/`、`out/`）与服务器运行时文件。
- 各项目 `deploy.sh` 默认部署到 `/home/ubuntu/minecraft`（Paper 服务器，tmux 会话 `paper`）；插件更新通常要重启服务器生效。
- 改完代码后按项目 README 的方式验证（构建 + 相关单测），再说"完成"。
- 客户端 mod / 安装包交付规范：
  - 产物文件名必须带**版本号**（如 `minea`+`audio-client-kit-26.2-0.1.0.zip`）
  - 只打包本插件自己的产物；如果由前置 mod 请只在第一次交给用户安装时提供，后续不要打包。
  - 构建后上传 temp.sh 并把下载链接发给用户：
    `curl -s -F "file=@<zip>" https://temp.sh/upload`
- 写好.gitignore，不要把服务器相关信息泄露公开，本AGENTS.md也不要提交到远程仓库。
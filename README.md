# minePlugins

jzk 服务器的插件合集，用 git submodule 汇总管理。每个子项目都是独立仓库，可以单独开发、单独发布。

```bash
git clone --recurse-submodules git@github.com:jurky123/minePlugins.git
```

## 插件一览

### MineAgent —— 服务器 AI 聊天助手

玩家在游戏聊天里 `@agent 提问`（或命令 `/agent`）就能和 AI 对话，回答全服可见。

- 查服务器状态、在线玩家、玩家坐标、世界时间与天气
- 记得最近聊过什么，可以追问
- 帮忙传送玩家、给物品、执行服务器命令——需要管理员点 **[批准]** 才会执行，且以请求者本人权限为准
- 所有操作有记录，可追溯

仓库：[jurky123/mineAgent](https://github.com/jurky123/mineAgent)

### MineChess —— 服务器国际象棋

两个人坐在真实的 3D 棋盘两边对弈，也支持 AI 对手、观战席和可选的 2D 棋盘界面。

- 完整规则：易位、吃过路兵、升变、将军/将死/逼和，50 回合 / 三次重复 / 子力不足判和，棋钟与断线重连
- 3D 实体棋盘：实体棋子、双方座位、棋钟、上一手/选中/将军高亮与动画，**右键**打开对局面板
- 2D 棋盘（MineUI）：固定视角鼠标点格子，两侧世界/对局聊天，升变直接在棋盘上选
- 每桌 4 个观战席：房间、大厅「进行中对局」或 `/chess spectate <玩家>` 加入，开局后坐到棋盘两侧
- 房间流程：大厅 / 房间列表 / 邀请 / 准备 / 切换执色 / 添加 AI，兼容 `/chess challenge` 快速开局
- 可插拔 AI：内置随机落子，实现 `com.minechess.ai.ChessBot` 即可替换
- 资源包由 PackHost 托管：3D 棋盘与棋子模型 + 2D 像素棋子（Lucas312，CC-BY 3.0）

仓库：[jurky123/mineChess](https://github.com/jurky123/mineChess)

### MineUNO —— 服务器 UNO 卡牌游戏

在服务器里玩 UNO：完整的 3D 实体牌桌与实体手牌，玩家不需要装客户端模组，配套材质包由 PackHost 自动托管分发。

- 完整经典规则：+2 / 跳过 / 反转 / 万能牌 / +4 质疑、UNO 喊牌与抓牌、牌库自动洗回
- 双模式：Quick（一局定胜负）与 Classic 500（官方计分累计 500 分），2~8 人多桌同时进行
- 3D 牌桌：桌面牌堆、弃牌堆随机叠放、发牌/摸牌/出牌动画、桌面显示当前颜色与方向
- 3D 手牌：只有自己可见、准星选牌、超过 16 张可翻页，另有 `/uno cards` + `/uno play` 兜底
- 回合提示：头顶回合标识、被跳过动画、BossBar 与音效粒子；大厅/房间/规则教程界面齐全
- 断线 60 秒内可重连，支持多个竞技场坐标与滚轮实时调参

仓库：[jurky123/mineUNO](https://github.com/jurky123/mineUNO)

### MineUI —— 自定义界面框架（Paper + Fabric）

一套服务端配 Fabric 客户端的自定义 UI 框架，突破原版 54 格箱子限制，用来承载菜单、游戏界面、HUD 等更自由的界面。

- 任意布局与常用控件：文本/按钮/图片、滚动列表、网格、模态弹窗、输入框、滑块、进度条、Tooltip
- HUD：服务端声明布局（锚点/偏移/缩放），与屏幕并存，客户端本地偏好与 F6 开关
- 原版风格模板：按钮/输入框/滑块/滚动条/面板/弹窗直接使用原版贴图，业务只写 `"skin": "vanilla:…"`
- 视觉与动画：圆角、描边、渐变、阴影、悬停过渡、点击/状态脉冲；进度低频率下发 + 客户端插值
- 3D 预览：物品（含卡面模型）、生物实体、玩家（在线皮肤或任意皮肤，可滚轮缩放）
- 装饰素材与动效：物品/头颅、原版精灵与像素图标，悬浮缩放/换图案、按时间轮换颜色或物品
- 远程图片：HTTPS 直连 + 服务端域名白名单 + 私网拦截 + 磁盘缓存，网易云封面等已验证
- 列表与歌词：`list` 节点（条目模板 + 高亮行 + 自动居中 + 滚轮）
- 短提示与全局动作：服务端 Toast（图标/时长），打开界面后点击可回传无会话动作
- 键位：客户端通用键位池 + 服务端声明，改键/冲突检测走原版按键设置
- 页面归业务插件：随界面会话下发声明式 JSON，MineUI 只负责解析、渲染与安全校验
- 业务 API `com.mineui.api`：会话、状态、动作与 owner 生命周期；客户端 mod 可选，不支持时业务回退原版界面
- 已被 MineSkin（皮肤浏览器）、MineChess（2D 棋盘/房间/聊天）与 MineAudio（音乐 UI）使用；规划中的业务：商店、任务、排行榜

仓库：[jurky123/mineUI](https://github.com/jurky123/mineUI)

### MineSkin —— 服务器皮肤定制包

换肤交给 SkinsRestorer，MineSkin 插件把 `/skins` 作为唯一皮肤入口（SR 自带 GUI 已禁用，无重复功能）。

- `/skins` 打开皮肤界面：顶部搜索栏、悬停条目即时 3D 预览、点击锁定/取消、一键应用或清除皮肤
- 原版客户端 / 旧版 mod 自动回退为可点击的聊天列表（关键词搜索 + 翻页）
- 内置 90+ 皮肤，中文界面 + 换肤冷却 5 秒；`/skin <名字>` 直接换肤
- 正版玩家进服自动恢复自己账号的皮肤
- 界面是随插件下发的声明式 JSON（玩家装 MineUI 0.7.0+ 即可，改界面不用重发 mod）
- `./deploy.sh` 一键构建并部署到 Paper 服务器（MineUI + MineSkin / 配置 / 皮肤）

仓库：[jurky123/mineSkin](https://github.com/jurky123/mineSkin)

### MineAudio —— 统一音频基础设施（音乐 / 环境音 / 游戏音效）

服务器级 **Audio Orchestrator**：统一管理"什么时候、给谁、在哪里、播放什么"，业务插件只依赖 `com.mineaudio.api`。

- 三种来源：资源包 OGG（位置声）、原版音效、音符盒 NBS（NoteBlockAPI）；流媒体由自研客户端直连音源，服务端不代理音频流
- 四种 Bus：MUSIC（每人一条）/ AMBIENT（多层）/ SFX / UI；范围：单玩家 / 全服 / 世界 / 区域（Cuboid、Sphere）/ 发声点
- 点歌与搜索：网易搜索、全服统一队列（自动续播）、客户端本地曲库（M2 进行中）
- MineUI 音乐界面：`/mineaudio ui` 查看/控制播放，`/mineaudio hud` 切换"正在播放" HUD；未装客户端时自动降级
- 事件与能力查询、Fallback 机制（资源包/NoteBlockAPI 缺失只影响对应 Backend）

仓库：[jurky123/mineAudio](https://github.com/jurky123/mineAudio)

### MineDisplay —— 图片与视频展示（规划中）

计划提供世界中的图片画框、公告墙与视频屏幕，以及类似侧边计分板的图片 HUD。

- 首期原版地图拼图，无需 mod 或资源包；后续复用 MineUI 图片 HUD 与管理页
- 可选 Fabric 客户端负责高清世界纹理和同步视频；原版视频默认降级为封面
- MineAudio / PackHost 按现有业务 API 或部署流程可选集成；跨项目 API 扩展仅记录需求
- 当前仅有设计文档，尚无可运行插件或客户端

仓库：[jurky123/mineDisplay](https://github.com/jurky123/mineDisplay) · [设计](mineDisplay/docs/DESIGN.md) · [实施计划](mineDisplay/docs/PLAN.md)

### MineRenderer / VoxelLight —— Vulkan 客户端路径追踪

Minecraft 26.2 / Java 25 / Fabric 客户端渲染器。alpha.45 修复默认 ROUGH_DIFFUSE 缓存覆盖：分离 diffuse/specular、B0/B1/B2 diffuse cache、精确镜面 continuation 与对应 PDF/MIS、diffuse history、间接训练请求和 generation 槽位回收。alpha.42 已实测 Vulkan 世界接管与 DLSS RR 运行，当前场景 FAST 输运耗时降低 23.01%，Iterative 增加 32.76%；默认保留 EXACT / Wavefront，FAST 按场景选择。alpha.44 专项三组均为波动内；alpha.44 默认 ROUGH_DIFFUSE 入口排除导致零查询/训练；alpha.45 修复已进入数值门禁，RTX 画质/生产性能仍待验收。[验收分析](https://github.com/jurky123/mineRenderer/blob/main/docs/performance/ALPHA-44-MATERIAL-ANALYSIS.md)。

- alpha.45 首份 production FULL 基线 4 段有效；GPU world P50 约 13.53–13.60 ms，缓存对照/画质仍待验收。[基线分析](https://github.com/jurky123/mineRenderer/blob/main/docs/performance/ALPHA-45-PRODUCTION-BASELINE.md)。
- alpha.46 修正首个 GPU 样本等待与稳定性超时混淆、断线取消原因；358 项测试通过。缓存性能退化及首次执行长停顿根因仍待 RTX 定位。[修复记录](https://github.com/jurky123/mineRenderer/blob/main/docs/performance/ALPHA-46-BENCHMARK-FIXES.md)。
- alpha.46 Material 24 段全部有效，PRIMARY 对 TAIL transport 改善 8.62%，FULL Split 退化 7.07%；缓存覆盖与路径减少已确认，生产 FULL 配对/画质仍待验收。[实测分析](https://github.com/jurky123/mineRenderer/blob/main/docs/performance/ALPHA-46-MATERIAL-ANALYSIS.md)。
- 构建：`cd mineRenderer && ./gradlew build clientKit`
- 验收：`/voxellight rt_reconstruction dlss`、`/voxellight rt_benchmark material`（专项 A/B）、`/voxellight rt_benchmark production`（保留日常配置，长期记录帧 P50/P95、CPU/GPU/内存）。
- [Material-Aware Cache 2.1、实现与验收](https://github.com/jurky123/mineRenderer/blob/main/docs/performance/MATERIAL-AWARE-CACHE-21.md)；[Primary / Cache 2.0、已实现项与限制](https://github.com/jurky123/mineRenderer/blob/main/docs/performance/PRIMARY-CACHE-2.md)；默认仍 MONOLITHIC/TAIL/OWEN/EXACT/Wavefront，新算法与复杂 DLSS motion 待 RTX 画质/性能验收。
- [实施、依赖与限制](https://github.com/jurky123/mineRenderer/blob/main/docs/performance/CAUSTICA-FRAME-PIPELINE.md)；仓库：[jurky123/mineRenderer](https://github.com/jurky123/mineRenderer)

## 目录

```text
minePlugins/
├── mineAgent/   # AI 聊天助手（Paper 插件 + 后端服务）
├── mineAudio/   # 统一音频基础设施（资源包/NBS/流媒体 + 业务 API）
├── mineDisplay/ # 图片/视频展示（规划：Paper + 可选 Fabric，复用 MineUI）
├── mineChess/   # 国际象棋（3D 棋盘 + MineUI 2D 棋盘/观战/聊天）
├── mineSkin/    # 皮肤定制包（MineSkin 插件 + SkinsRestorer 配置 + 内置皮肤）
├── mineUNO/     # UNO 卡牌游戏（Paper 插件 + 资源包托管）
└── mineUI/      # 自定义 UI 框架（Paper 插件 + Fabric 客户端模组）
```

## 开发说明

- 每个子目录是独立仓库：修改、提交、推送都在对应仓库中进行
- 汇总仓库只记录各插件的版本指针；插件更新后，在本仓库执行
  `git submodule update --remote` 即可同步到最新版本并提交
- 只想拉取某个插件：`git clone git@github.com:jurky123/<插件名>.git`

## 2026-10-01 审查修复

- MineChess 1.0.1：落子与升变立即核验棋钟，超时无法通过加秒恢复；保留 AI 不判超时策略。
- MineUNO / PackHost 1.0.1：无关玩家退出不取消待决策；资源包重建串行化、独立临时文件，下载摘要与内容一致。
- MineAudio 0.5.5：区域迟滞按实际经过的 server tick 推进，不再被检测间隔成倍放大。
- MineUI 0.16.1：远程图片纹理按 128 MiB 预算淘汰不活跃项；客户端须更新 mod 才能获得修复。
- MineAgent：HTTP 房间快照不共享可变状态，悔棋失效旧解说，终局在房间锁外写 SQLite。

各项目回归测试覆盖上述边界；MineUI 纹理释放仍需客户端实机验证。汇总仓库子模块指针保持不变。

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

- 任意布局与常用控件：文本/按钮/图片、滚动列表、网格、模态弹窗、输入框、Tooltip
- 视觉与动画：圆角、描边、渐变、阴影、悬停过渡、点击/状态脉冲
- 3D 预览：物品（含卡面模型）、生物实体、玩家（在线皮肤或任意皮肤）
- 页面归业务插件：随界面会话下发声明式 JSON，MineUI 只负责解析、渲染与安全校验
- 业务 API `com.mineui.api`：会话、状态、动作与 owner 生命周期；客户端 mod 可选，不支持时业务回退原版界面
- 已被 MineSkin（皮肤浏览器）使用；规划中的业务：UNO 界面、商店、任务、排行榜、HUD

仓库：[jurky123/mineUI](https://github.com/jurky123/mineUI)

### MineSkin —— 服务器皮肤定制包

换肤交给 SkinsRestorer，MineSkin 插件把 `/skins` 作为唯一皮肤入口（SR 自带 GUI 已禁用，无重复功能）。

- `/skins` 打开皮肤界面：顶部搜索栏、悬停条目即时 3D 预览、点击锁定/取消、一键应用或清除皮肤
- 原版客户端 / 旧版 mod 自动回退为可点击的聊天列表（关键词搜索 + 翻页）
- 内置 90+ 皮肤，中文界面 + 换肤冷却 5 秒；`/skin <名字>` 直接换肤
- 正版玩家进服自动恢复自己账号的皮肤
- 界面是随插件下发的声明式 JSON（玩家装 MineUI 0.6.6+ 即可，改界面不用重发 mod）
- `./deploy.sh` 一键构建并部署到 Paper 服务器（MineUI + MineSkin / 配置 / 皮肤）

仓库：[jurky123/mineSkin](https://github.com/jurky123/mineSkin)

## 目录

```text
minePlugins/
├── mineAgent/   # AI 聊天助手（Paper 插件 + 后端服务）
├── mineChess/   # 棋类游戏插件
├── mineSkin/    # 皮肤定制包（MineSkin 插件 + SkinsRestorer 配置 + 内置皮肤）
├── mineUNO/     # UNO 卡牌游戏（Paper 插件 + 资源包托管）
└── mineUI/      # 自定义 UI 框架（Paper 插件 + Fabric 客户端模组）
```

## 开发说明

- 每个子目录是独立仓库：修改、提交、推送都在对应仓库中进行
- 汇总仓库只记录各插件的版本指针；插件更新后，在本仓库执行
  `git submodule update --remote` 即可同步到最新版本并提交
- 只想拉取某个插件：`git clone git@github.com:jurky123/<插件名>.git`

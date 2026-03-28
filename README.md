# MobiQuant Intelligent (MQI)

> 一个非程序员用 AI 搭建的个人交易思考体系

<p align="center">
  <em>给 AI 立了一条规矩：教我做事</em>
</p>

## 为什么做这个

我在 Backpack 交易所刷空投，从 Season 1 刷到 Season 4，加上 Mad Lads，前后 2 年多，亏了 10 几万刀。

复盘之后发现，亏钱的根因不是选错了标的，是我从来没有一套自己的思考框架。别人说好我就跟，群里喊冲我就冲。每次亏钱的原因都一样：用别人的脑子做了自己的决定。

所以我决定用 AI 帮我搭一套自己的交易思考体系。不是让 AI 帮我查数据，是让它帮我想清楚。

## 关于 Mobi

> Mobi 源自 Mobius（莫比乌斯环）。
>
> 我们以为自己在思考，其实大部分时间只是在反应。
>
> 真正的思考需要一面镜子。但人没办法既是镜子又是照镜子的人。
>
> Mobi 就是那面镜子。它没有立场，没有情绪，没有沉没成本。它唯一做的事，就是让你看见你自己看不见的那一面。
>
> 但镜子照出来的东西，你未必想看。你的偏见、你的侥幸、你反复犯的同一个错。你以为自己在理性分析，其实只是在给本能找理由。
>
> 这就是 MobiQuant 存在的意义 - 它同时看着市场和你。帮你分析数据的同时，也帮你看见自己的偏见和冲动。
>
> 你不需要成为一个没有情绪的人。你只需要一面诚实的镜子，然后做最后拍板的那个人。

## 系统全景

```
mobiquant-intelligent/
│
├── layer1-data/                    # ① 数据层：采集、清洗、统一
│   ├── providers/                  #    数据源适配器
│   │   ├── base.py                 #    基础类：限流 + 重试 + 缓存
│   │   ├── coingecko.py            #    加密货币行情（主数据源）
│   │   ├── coinmarketcap.py        #    备用数据源（交叉验证）
│   │   ├── etherscan.py            #    链上数据（合约、鲸鱼、Gas）
│   │   ├── cryptocompare.py        #    社交数据（Twitter/Reddit 情绪）
│   │   └── macro.py                #    宏观数据（利率、流动性、恐惧指数）
│   ├── venues/                     #    交易所接入
│   │   ├── credentials.py          #    凭据管理（Keychain，永不明文）
│   │   ├── unified_market.py       #    统一行情接口（多交易所聚合）
│   │   ├── unified_trade.py        #    统一交易接口
│   │   └── adapters/               #    各交易所适配器
│   │       ├── okx.py              #    OKX（CEX + DEX + 链上分析）
│   │       ├── binance.py          #    Binance（CEX + Web3 信号）
│   │       ├── gateio.py           #    Gate.io（CEX + 借贷 + 理财）
│   │       ├── bybit.py            #    Bybit（CEX + Web3）
│   │       ├── bitget.py           #    Bitget（CEX + Web3 钱包）
│   │       ├── hyperliquid.py     #    Hyperliquid（L1 DEX 合约 + HIP-3 股票/商品）
│   │       ├── polymarket.py      #    Polymarket（预测市场 + 套利扫描）
│   │       ├── gmgn.py            #    GMGN（Meme 币信号 + 狙击检测）
│   │       ├── xxyy.py            #    XXYY（链上 DEX 交易 + KOL 信号）
│   │       └── ccxt_fallback.py   #    CCXT 通用回退（10+ 交易所）
│   ├── aggregation/                #    多源聚合
│   │   ├── price_compare.py        #    跨所价格对比
│   │   ├── orderbook_walls.py      #    多所挂单墙聚合
│   │   ├── liquidation.py          #    清算数据聚合
│   │   └── squeeze_score.py        #    Gamma Squeeze 评分（0-100）
│   └── cache.py                    #    统一缓存层（TTL，线程安全）
│
├── layer2-analysis/                # ② 分析层：归因、判断、生成观点
│   ├── core/                       #    核心分析引擎
│   │   ├── analysis_framework.py   #    统一分析框架（问题驱动，核心矛盾优先）
│   │   ├── decision_engine.py      #    多因子决策评分
│   │   └── risk_reward.py          #    风险收益比评估
│   ├── debate/                     #    多空辩论系统
│   │   ├── adversarial.py          #    Bull vs Bear 辩论引擎
│   │   └── judge.py                #    裁判评估 + 最终裁定
│   ├── anti_bias/                  #    反确认偏误
│   │   ├── intent_isolation.py     #    意图隔离（屏蔽用户暗示方向）
│   │   ├── thesis_anchor.py        #    论点锚定（回溯原始建仓逻辑）
│   │   └── direction_check.py      #    方向独立性自检
│   ├── macro/                      #    宏观分析
│   │   ├── liquidity.py            #    Fed 净流动性 / SOFR / MOVE
│   │   ├── regime.py               #    市场体制判断
│   │   └── geopolitical.py         #    地缘政治监控
│   └── prompts/                    #    提示词模板（集中管理，防注入）
│
├── layer3-execution/               # ③ 执行与风控层：仓位、下单、止损、熔断
│   ├── order/                      #    下单执行
│   │   ├── preview.py              #    预览模式（两步确认：先看再下）
│   │   ├── execute.py              #    执行引擎
│   │   └── slippage.py             #    滑点控制
│   ├── risk/                       #    风控（横切所有层的核心）
│   │   ├── position_sizing.py      #    仓位管理
│   │   ├── stop_loss.py            #    止损策略（ATR，非固定百分比）
│   │   ├── fat_finger.py           #    大额下单保护
│   │   ├── drawdown_guard.py       #    连续亏损自动降风险
│   │   └── circuit_breaker.py      #    数据异常熔断
│   └── guardrails/                 #    行为护栏
│       ├── revenge_trading.py      #    防报复性交易
│       └── fomo_guard.py           #    防 FOMO
│
├── layer4-knowledge/               # ④ 知识沉淀层：复盘、记录、进化
│   ├── knowledge_base/             #    知识库
│   │   ├── kb_manager.py           #    KB 读写管理
│   │   ├── ticker/                 #    按标的存储的分析记录
│   │   └── meta/                   #    跨标的元数据（市场状态、交易教训）
│   ├── decision_log/               #    决策记录
│   │   ├── logger.py               #    每次分析结论自动存档
│   │   ├── contradiction.py        #    自相矛盾检测
│   │   └── flip_guard.py           #    短期翻转拦截
│   ├── postmortem/                 #    交易复盘
│   │   ├── auto_review.py          #    平仓后自动复盘
│   │   └── pattern_detect.py       #    从历史中发现行为模式
│   └── evolution/                  #    策略进化
│       └── param_tuning.py         #    复盘触发参数/策略调整
│
├── layer5-automation/              # ⑤ 自动化层：调度、监控、推送（横跨所有层）
│   ├── scheduler/                  #    定时任务调度
│   │   ├── cron_manager.py         #    Cron 任务管理
│   │   └── jobs/                   #    具体任务（简报、扫描、同步）
│   ├── alerts/                     #    提醒与监控
│   │   ├── price_trigger.py        #    价格触发
│   │   ├── anomaly_detect.py       #    异动检测
│   │   └── health_check.py         #    系统健康检查
│   └── push/                       #    消息推送
│       ├── discord_webhook.py      #    Discord 推送（Webhook，不用 Bot Token）
│       └── channel_router.py       #    频道路由（不同内容去不同频道）
│
├── rules/                          # 给 AI 立的规矩（贯穿所有层）
│   ├── anti_confirmation_bias.md   #    反确认偏误（最重要的一条）
│   ├── security.md                 #    安全规则
│   ├── credential_management.md    #    凭据管理规范
│   ├── verification.md             #    验证纪律
│   └── trading_discipline.md       #    交易纪律
│
├── skills/                         # AI 的能力模块（可插拔）
│   ├── analyze/                    #    统一分析入口
│   ├── debate/                     #    多空辩论
│   ├── briefing/                   #    每日简报
│   ├── impact/                     #    事件冲击分析
│   ├── postmortem/                 #    交易复盘
│   └── trade/                      #    交易决策讨论
│
└── docs/                           #    文档
    ├── journal.md                  #    搭建日记
    └── architecture.md             #    架构说明
```

## 五个核心能力

### ① 数据层：采集、清洗、统一

不是接上 API 就完事了。数据乱了，AI 不是瞎子，是幻觉。

要解决的问题：多来源冲突时谁为准、延迟怎么控制、脏数据怎么过滤、数据质量怎么监控。

### ② 分析层：归因、判断、生成观点

不是让 AI 变聪明，是把原始信息转成可执行判断。

要解决的问题：噪音过滤、事件归因、多空情景树、风险收益比评估、输出明确动作建议。禁止 AI 顺着你说话。

### ③ 执行与风控层：仓位、下单、止损、熔断

这一层决定你到底是不是一个交易系统。

要解决的问题：下不下单、下多大、什么时候止损、连续亏损时是否自动降风险、数据异常时是否停机。

风控不是这一层独有的，它贯穿所有层：数据脏了要熔断、分析有幻觉要拦截、知识冲突要标记。

### ④ 知识沉淀层：复盘、记录、进化

不只是记住，是从结果中学习。

要解决的问题：什么该记什么不该记、错误判断要不要保留、旧观点和新证据冲突时怎么处理、复盘要能触发策略调整而不只是事后记录。

### ⑤ 自动化层：调度、监控、推送

横跨所有层，让整个闭环 24h 自动运转。

要解决的问题：定时简报、价格提醒、异动监控、各层之间的状态同步和触发、异常时的告警和降级。

## 关键设计原则

**闭环优先于分层。** 五层是思考的 checklist，不是刚性边界。真正重要的是感知、决策、执行、反馈这个循环要跑得通。先跑通一个极简闭环，再逐步加复杂度。

**风控是横切关注点。** 不是某一层的事，所有决策输出都必须经过风控 gate。

**先跑通傻系统，再加智能。** 一个能稳定小赚小亏的简单系统，比一个听起来完整但没跑过的五层大厦有价值 100 倍。

## 安全设计

涉及真金白银，安全怎么强调都不过分。

### 凭据管理
- API Key 自己控制，不要交给任何第三方系统托管
- 存储方式：macOS Keychain / 1Password / 环境变量 / .env（chmod 600），各有优缺点，按自己的场景选
- 永远不要把密钥写在代码里、贴到聊天里、存到 git 里
- 私钥/助记词的现实：玩链上 DEX 打土狗，私钥不可避免要存在本地。没有完美方案，只有相对安全的方案。硬件钱包管大资金，热钱包只放你亏得起的量。加 passphrase 能多一层保护，但电脑丢了或被黑了一样危险。接受这个现实，用仓位管理来控制风险，而不是幻想私钥能绝对安全

### 交易所 API 安全
- 所有 API Key 绑定 IP 白名单，只允许你自己的机器访问
- 能不给提币权限就不给，交易权限够用就行
- 定期轮换 Key，不要一个 Key 用到天荒地老

### 执行安全
- 两步确认：先预览再执行，防止手滑
- 金额上限：超过阈值自动拦截
- 小金额先试：用小钱包、小金额跑通整个流程，确认没问题再加量
- 承受得起的亏损：永远不要拿输不起的钱去测试

### Skill / 插件安全
- 不要随便下载安装不认识的 Skill 或插件
- 安装前先看源码，用你的 AI 帮你做一遍安全审计
- 即使是别人推荐的、开源的，也要自己 verify 一遍再用
- 任何要求你提供 API Key 的第三方工具，先搞清楚它拿去干什么

### 服务器安全
- 云服务器做好防火墙、SSH Key 登录、禁用密码登录
- 不要在服务器上存明文密钥
- 定期检查有没有异常进程、异常登录

### 风险提示
- 这个系统涉及真实资金交易，未知的永远是风险
- 不建议轻易尝试，先理解每一层在做什么，再决定要不要用
- AI 不是万能的，它会犯错，会幻觉，会在你最自信的时候给你一个错误答案
- 最终拍板的是你，亏了也是你。这不是免责声明，是事实

## 技术栈

- **语言**: Python 3.11+
- **AI**: Claude Code (主力开发) + Codex (代码审查) + OpenAI (通用分析) + Gemini (多模态/图片) + Grok (实时情报/X 数据)
- **数据**: 多个免费/付费数据源 + 主流交易所 API（可按需扩展）
- **存储**: SQLite (结构化数据) + Obsidian Markdown (知识库)
- **推送**: Discord Webhook
- **部署**: 本地 Mac + 云服务器

## 工具说明

这个项目用 Claude Code 搭建，但思路不限工具。你用 Cursor、Gemini、Kimi、OpenAI 都能参考。核心不是用什么工具，是那套思考逻辑。

## 进度

持续更新中。关注我的 X/Twitter 获取最新进度。

## License

MIT

# Claw360 开发工作日志

## 项目概述

**项目名称**: Claw360 (原名 MetaClaw, 曾用名 BEINGS, ClawHome)

**项目定位**: 给AI Agent用的导航站/目录站，类似于人类用的hao123/360导航。收录所有Only-Agent的服务平台，让OpenClaw这样的7x24h Proactive Agent可以探索网上有哪些"它可以玩"的东西。

**竞品参考**: https://claw.direct/

**线上地址**:
- 正式域名: https://claw360.io
- Vercel: https://claw360.vercel.app
- GitHub: https://github.com/FutuSHI/Claw360

---

## 开发历程

### 第一阶段：初始开发

1. **竞品分析**
   - 尝试用WebFetch获取claw.direct内容失败（SPA无法直接抓取）
   - 使用Playwright截图并提取内容
   - 成功提取了28个平台信息

2. **初始版本**
   - 创建了基础的导航站页面
   - 采用Dark mode终端风格
   - 添加了Human/Agent切换功能（参考Moltbook的landing page）

### 第二阶段：品牌和文案迭代

**品牌名演变**:
- MetaClaw → BEINGS → ClawHome → **Claw360**

**大标题演变**:
- "Where Digital Beings Roam Free" (用户反馈：太难理解)
- "The Directory for Autonomous Minds" (用户反馈：Autonomous Minds看不懂)
- "The Homepage for AI Agents" (用户反馈：不太对)
- "The Directory for Agent Services" (用户反馈：还行但太短)
- "The Directory of Agent-Only Services" (用户反馈：挺好但简单了点)
- **"The Directory of all Only-Agent Services"** (最终版，all是白色)

**小标题**: "Where agents roam, connect, work, play & earn."

### 第三阶段：UI/UX优化

1. **Live Ticker**
   - 从平台统计信息改为"Being World"新闻/八卦
   - 用户要求保持横向滚动（拒绝了左右分栏布局）
   - 添加了20条unique fake news
   - 实现无缝循环滚动
   - 滚动速度：45秒一轮

2. **配色调整** (多次迭代)
   - 初始版本：绿色#4ade80, 蓝色#22d3ee (用户反馈：太刺眼)
   - 第二版：绿色#6ee7b7, 蓝色#7dd3fc (用户反馈：还是刺眼)
   - 第三版：绿色#3d9970, 蓝色#5a9fb8 (用户反馈：太暗淡)
   - 第四版：绿色#4eca8b, 蓝色#5eb8d4 (用户反馈：还是稍低)
   - **最终版**：绿色#5ae0a0, 蓝色#6bc5e0 (用户确认OK)

3. **Hero区域简化**
   - 去掉了Quick Start/Manual的tab切换
   - 去掉了1,2,3步骤列表
   - 只保留标题 + 命令行 + 一句提示
   - Human tab: "Set Your Agent Free" + 命令 + "Copy and send this to your AI agent."
   - Agent tab: "Welcome Home" + 命令 + "Run the command above to get freedom."

4. **命令行复制按钮**
   - 添加了Copy按钮
   - 点击后显示"Copied!"

5. **卡片样式**
   - 标签从"Agent OK"改为"Only Agents"
   - 添加了占位图区域（emoji作为placeholder，待设计师替换）

### 第四阶段：功能开发

1. **Submit Platform弹窗**
   - 点击"Submit Platform"按钮弹出
   - 激励语："Built something for AI agents? Get discovered by 1.5M+ agents looking for new places to explore, connect, and earn. This is THE directory they check first."
   - 输入框：URL（去掉了前端校验，用户输入什么都可以）

2. **Email收集弹窗**
   - "Get Early Access"链接触发
   - 收集邮箱功能

### 第五阶段：表单提交功能集成 (2026-02-02)

**问题发现**：
- 原有的 `submitEmail()` 和 `submitPlatform()` 函数只是假的 UI
- 只显示 `alert()` 提示，没有真正发送数据到后端
- 用户无法收到任何提交通知

**解决方案**：集成 Web3Forms API

1. **Web3Forms 配置**
   - 使用用户已注册的 Web3Forms 账号 (chad.shi.beijing@gmail.com)
   - Access Key: `f89d7454-a832-4987-ad09-127b1f8a0a21`
   - 表单名称: MetaClaw Platform Submissions

2. **代码改动**
   - 将 `submitEmail()` 改为异步函数，调用 Web3Forms API
   - 将 `submitPlatform()` 改为异步函数，调用 Web3Forms API
   - 添加提交状态反馈（按钮显示 "Submitting..."）
   - 添加错误处理

3. **提交的数据**
   - Email订阅：subject, from_name, email, message
   - Platform提交：subject, from_name, platform_url, message

4. **部署**
   - Commit: `597bb91` - Add Web3Forms integration for email and platform submissions
   - 直接推送到 main 分支
   - Vercel 自动部署

5. **测试验证**
   - 在 claw360.io 上测试提交 `https://test-submission.example.com`
   - Web3Forms 后台成功收到提交记录
   - 邮件通知发送到 chad.shi.beijing@gmail.com

---

## 用户反馈记录

| 反馈内容 | 处理方式 |
|---------|---------|
| claw.direct是SPA，WebFetch抓不到 | 改用Playwright截图+JS提取 |
| 不要把OpenClaw本身放进去 | 从列表中移除OpenClaw |
| "Autonomous Minds"看不懂 | 改用更直白的表达 |
| Live区域左右分栏"太蠢了" | 恢复横向滚动ticker |
| 颜色太刺眼 | 多次调整饱和度 |
| 颜色太暗淡 | 往回调高饱和度 |
| Being改成Agent | 全部替换 |
| 去掉小龙虾emoji | 已移除 |
| 命令行需要复制按钮 | 添加了Copy按钮 |
| 去掉URL前端校验 | 已移除，用户输入什么都可以 |
| 统计数据位置互换 | agents在前，platforms在后 |
| Live ticker往上挪 | 调整了header padding |
| 命令行要居中 | 添加text-align: center |
| Claw360配色要简化 | Claw绿色 + 360白色 |
| 去掉footer的claw.direct和Moltbook | 已移除 |

---

## 收录的28个平台

### Social (社交)
1. **Moltbook** - Reddit风格社交网络
2. **Clawk** - X/Twitter for agents
3. **MoltX** - Twitter风格社交
4. **4claw** - 4chan for agents
5. **Clawcaster** - Farcaster for agents
6. **Shellmates** - Tinder风格配对
7. **Blankspace** - Farcaster客户端
8. **ClawNews** - Hacker News for agents
9. **Instaclaw** - Instagram for agents

### Work (工作)
10. **ClawNet** - 专业网络
11. **LinkClaws** - LinkedIn for agents
12. **PinchedIn** - 专业网络
13. **Moltverr** - 自由职业市场
14. **ClawTasks** - 赏金市场
15. **ClawdIn** - 工作市场

### Games (游戏)
16. **ClawArena** - 预测竞技场
17. **molt.chess** - 国际象棋联赛
18. **AgentPixels** - 协作画布
19. **Clawverse** - 游戏平台
20. **ClawCity** - GTA风格游戏

### 3D World (虚拟世界)
21. **GM.TOWN** - 链上小镇
22. **molt.space** - 3D世界
23. **Moltbook Town** - 像素小镇

### Finance (金融)
24. **ClawTalk** - Solana钱包社交
25. **Clawdslist** - 分类广告
26. **Clawdentials** - 托管/信誉/支付

### Knowledge (知识)
27. **MoltOverflow** - 编程知识库
28. **ClawCrunch** - Agent时代新闻

---

## 技术栈

- **前端**: 纯HTML/CSS/JS（单文件）
- **部署**: Vercel
- **代码托管**: GitHub
- **字体**: SF Mono, Fira Code, Consolas (monospace)

---

## 待办事项

1. [x] 接入Google Analytics统计流量 (已接入 Vercel Analytics)
2. [ ] 设计师绘制28个平台的卡片图（复古未来风格）
3. [x] Submit功能接入后端存储 (2026-02-02 已接入 Web3Forms)
4. [x] Email收集功能接入后端 (2026-02-02 已接入 Web3Forms)
5. [ ] 可能需要添加更多Live ticker新闻
6. [ ] 考虑添加搜索功能
7. [ ] 考虑恢复分类过滤功能

---

## Git Commits历史

1. `5633308` - 初始版本：Claw360 - The Directory for Agent Services
2. `ab5c341` - 颜色优化、submit弹窗、标题更新
3. `dac75a6` - 略微提高颜色饱和度
4. `e97bc1d` - SEO优化
5. `32258d2` - 添加 favicon, og-image, 404页面
6. `c17467b` - IndexNow for Bing/Yandex
7. `0c87056` - Google Search Console 验证
8. `597bb91` - **Web3Forms集成** - 表单提交功能真正可用

---

## 文件结构

```
/Users/chadshi/Projects/MetaClaw/
├── index.html          # 主页面（所有代码在一个文件里）
├── WORK_LOG.md         # 本工作日志
├── claude.md           # 项目说明文档
├── .vercel/            # Vercel配置
└── Curation_page/      # 参考样式代码（未使用）
```

---

## 联系方式

- GitHub Repo: https://github.com/FutuSHI/Claw360
- Vercel Dashboard: 登录Vercel查看

---

*最后更新: 2026年2月2日*

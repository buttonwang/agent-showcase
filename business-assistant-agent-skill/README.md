# 业务助手智能体 (Business Assistant Agent)

> 训练企业员工用 Codex 开发网页应用,解决具体业务问题。
> 零代码,几轮对话,出 PRD + 可用 Demo,持续迭代。

## 这是什么

一个 Codex Skill,帮企业员工把模糊的业务问题一步步变成可用的网页应用。

学员不需要写代码,只需要用自然语言描述业务需求。Skill 会自动生成:

1. **产品文档(PRD)**:写文件,可分享给同事审阅
2. **网页 Demo**:单文件 HTML,双击即开,数据存浏览器本地
3. **持续迭代**:可以一直改 PRD / 改应用,支持 4 种典型往返场景

适用场景:

- **培训**:企业员工几小时就能上手,做出自己业务部门的工具
- **内部工具**:替代 Excel / 微信群,几分钟搭一个原型
- **业务验证**:新想法做个 demo 跑 2 周,再决定要不要正式开发

## 装上 Codex 怎么用

### 1. 复制到 Codex Skill 目录

```bash
cp -r business-assistant-agent $CODEX_HOME/skills/
```

### 2. 启动 Codex,在对话里说

> 我是 HR 专员,平时用 Excel 管候选人太乱了,想搞个工具

或者直接说:

> 用业务助手智能体做一个销售拜访管理工具

### 3. 跟着 Skill 走 4 个阶段

- **阶段一** 需求发现(Skill 会问 2-3 轮)
- **阶段二** PRD 自动生成(Skill 写到文件,你审阅)
- **阶段三** Demo 自动生成(单文件 HTML,你打开看)
- **阶段四** 持续迭代(改 PRD / 改对话 / 改应用,一直循环)

## 文件结构

```
business-assistant-agent/
├── SKILL.md                      # Skill 主文档(给 Codex 读)
├── CHANGELOG.md                  # 版本历史
├── README.md                     # 本文件
├── agents/openai.yaml            # Skill 清单(触发词 + 描述)
├── assets/                       # 模板 + 设计 token
│   ├── prd-template.md
│   └── design-tokens.json
├── references/                   # Skill 内部知识库(7 个 .md)
├── examples/                     # 学员材料 + 讲师材料
│   ├── README.md                 # 学员快速开始
│   ├── walkthrough-hr-...md      # 完整演练(老师讲你看)
│   ├── student-checklist.md      # 作业 checklist(自己做)
│   ├── instructor-guide.md       # 讲师课程大纲
│   └── instructor-rubric.md      # 讲师评分量表
├── scripts/                      # 辅助脚本
│   ├── render_prd.py
│   └── validate_prd.py
└── work/
    └── smoke_test.py             # 静态校验(78 项)
```

## 给学员

- [examples/README.md](examples/README.md) - 2 分钟快速开始
- [examples/walkthrough-hr-recruitment.md](examples/walkthrough-hr-recruitment.md) - 22KB 完整演练
- [examples/student-checklist.md](examples/student-checklist.md) - 142 项作业 checklist

## 给讲师

- [examples/instructor-guide.md](examples/instructor-guide.md) - 1-2 天课程大纲
- [examples/instructor-rubric.md](examples/instructor-rubric.md) - 100 分制评分量表

## 验证安装

```bash
cd $CODEX_HOME/skills/business-assistant-agent
python work/smoke_test.py
```

期望输出:`总计: 78/78`

## 3 个参考 Demo

在 Skill 自带的 `outputs/` 下(讲师分发时一起打包)。每个 demo 都已带真实种子数据(销售 4 条拜访、HR 4 个需求 + 6 个候选人 + 3 场面试 + 3 个入职、会议 5 场 + 9 个行动项),双击 `index.html` 即可看到完整业务效果,无需先造数据。

| Demo | 业务场景 | 关键看点 |
| --- | --- | --- |
| `demo-sales-visit-recorder/` | 销售拜访纪要 | 单实体 CRUD + 看板拖拽 + 客户地图 |
| `demo-hr-recruitment/` | HR 招聘后台 | 多实体(招聘需求/候选人/面试/入职)+ 5 列管道看板 + 漏斗图 |
| `demo-meeting-notes/` | 会议纪要管理 | 多实体(会议/行动项)+ 5 种会议类型 + 行动项看板 |

每个 demo 都是单文件 HTML + README.md,双击即开,无任何依赖。

## 技术原理(1 分钟版本)

- 生成内容:LLM 直接写单文件 HTML
- CSS:Tailwind(CDN)
- 交互:Alpine.js 3(CDN)
- 图标:lucide(CDN)
- 存储:浏览器 localStorage
- 字体:Inter / JetBrains Mono(Google Fonts)

学员不需要懂任何代码。如果想看懂实现,所有 HTML 文件都可以在浏览器「查看源代码」看到完整结构。

## 常见问题

**Q:学员装 Skill 失败怎么办?**

A:让学员跑 `python work/smoke_test.py`,78/78 通过就是装好了。失败的话看具体哪 26 项不过。

**Q:生成出来的 Demo 怎么部署?**

A:把 `outputs/<产品名>/index.html` 单文件直接发到内网(Confluence / 钉钉 / 飞书),或扔到 Vercel / Netlify 静态托管,链接发同事。

**Q:学员数据丢了怎么办?**

A:数据在浏览器 localStorage 里,清浏览器缓存会丢。教学员在「设置」页用「导出数据」按钮定期备份。

**Q:Skill 能做原生 App / 后端 / 多人协作吗?**

A:不能。本 Skill 范围是「单浏览器 + localStorage + 单文件 HTML」。要做上述能力需要另外的 Skill 或后端。

## 更多信息

- 详细工作流:看 `SKILL.md`
- 4 场景路由规则:看 `references/iteration-patterns.md`
- CRUD 实现规范:看 `references/crud-pattern.md`

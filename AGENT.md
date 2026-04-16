# AGENT.md — CampusShare Storage 项目宪法

> **任何 AI agent（Claude Code / ChatGPT / 其他）打开此项目时，必须先完整读完本文件，再开始任何写作、修改、建模工作。**
> **本文件是项目唯一的"真相来源（single source of truth）"。任何章节草稿、图、幻灯片若与本文件冲突，以本文件为准；若需要调整，先修改本文件再传播到下游。**

---

## 1. 项目一句话定位

**CampusShare Storage** 是一个基于大学 `.edu` 身份验证的**校园内 P2P 短期存储交易平台**，连接暑期离校需要临时存储的学生（Renter）与拥有闲置空间的在校学生（Host），通过平台抽成盈利。

- **课程**：MIDS301 Introduction to Information — A Systems and Design Approach（Spring 2026）
- **作业总分**：40 分
- **业务模式**：Peer-to-peer marketplace / Sharing economy
- **核心战略**：**Focused Differentiation**（聚焦差异化）
- **核心竞争壁垒**：`.edu` 身份信任 + 步行可达的超短距离 + 双向评价体系

---

## 2. 关键时间节点

| 日期 | 事件 |
|---|---|
| 2026-04-15（今天）| 工作区搭建完毕 |
| 2026-04-16 | Task 2 初稿 + ERD 规范 |
| 2026-04-17 | ERD 定稿 + Task 3 |
| 2026-04-18 | BPMN 定稿 + Task 4 + 价值链表 |
| 2026-04-19 | Task 1 + Task 5 + PPT 大纲 |
| 2026-04-20 | 合稿 + 彩排 |
| **2026-04-21 或 04-23** | **课堂演讲（10 分钟）** |
| 2026-04-24 – 04-26 | 报告润色 |
| **2026-04-27 23:59** | **提交最终报告** |

---

## 3. 目标客户与使用场景

### 主客户：Renter（出资方）
- **Primary**：国际学生，暑期 2–3 个月回国或实习，宿舍物品无处存放
- **Secondary**：搬宿舍间歇期（学期交接）、短期实习外派、旅行

### 主客户：Host（供给方）
- **Primary**：在校学生，家里/公寓/宿舍有闲置空间（衣柜、储藏间、车库）
- **Secondary**：研究生公寓、合租房空房间

### 共同特征
- 都是 **同一所大学**的认证学生（`.edu` 邮箱）
- **彼此信任基于学校身份**，非陌生人

---

## 4. 核心假设（不可随意推翻）

1. **单一大学范围**：业务建模只覆盖一所大学，不建模跨校
2. **学生双身份**：同一个 Student 实体可同时是 Host 和 Renter，**不拆分为两个实体**
3. **第三方支付为黑盒**：支付通过 Stripe/PayPal，不建模支付风控细节
4. **.edu 邮箱即身份**：不建模学生证、校园卡等其他身份证明
5. **短期租期为主**：1 天 – 3 个月，不建模年度租赁
6. **物品类型不限**：不对特定品类（易碎品、违禁品）做特殊建模（可在 Task 5 提为局限）
7. **平台不物理托管物品**：仅撮合交易，Host 自行保管

---

## 5. 核心术语统一表（所有文档必须一致）

| 术语 | 含义 | 禁用替代词 |
|---|---|---|
| **Student** | 平台用户（学生），可同时为 Host 和 Renter | ~~User / Customer~~ |
| **Host** | 提供存储空间的 Student 角色 | ~~Provider / Lender / Owner~~ |
| **Renter** | 使用存储空间的 Student 角色 | ~~Customer / Buyer / Guest~~ |
| **StorageListing** | Host 发布的房源 | ~~Space / Unit / Room / Spot~~ |
| **Booking** | Renter 发起的预订订单（关联实体） | ~~Order / Reservation~~ |
| **StoredItem** | 入库的物品记录 | ~~Item / Thing / Package~~ |
| **Review** | 双方互评（关联实体） | ~~Rating / Feedback（narrative 里可用 feedback）~~ |
| **Dispute** | 纠纷记录 | ~~Complaint / Issue~~ |
| **Payment** | 支付记录 | ~~Transaction / Charge~~ |
| **University** | 大学实体，用于邮箱域校验 | ~~School / Campus~~ |
| **InsurancePolicy** | 可选保险附加 | ~~Coverage / Protection~~ |

---

## 6. 字数预算（总 3000 词 ±10%，硬上限 3300）

| Task | 推荐字数 | 累计 |
|---|---|---|
| Task 1 Executive Summary | 300 | 300 |
| Task 2 Strategy & Value Proposition | 1300 | 1600 |
| Task 3 Information Design | 450 | 2050 |
| Task 4 Business Execution | 450 | 2500 |
| Task 5 Critical Reflection | 500 | 3000 |
| **合计** | **3000** | ✅ |

**字数不包括**：references、appendices、Title Page、表格内容（但正文段落算）。

---

## 7. 满分硬性要求速查（按 task）

### Task 2（9 分） — Porter + 战略
- [ ] 五力**全部**覆盖（Entrants / Suppliers / Buyers / Substitutes / Rivalry）
- [ ] 每一力**点名具体竞争对手或数据**（U-Haul, Public Storage, Neighbor.com 等）
- [ ] 明确声明 **Focused Differentiation**，并解释为什么不选 Cost Leadership / Broad Differentiation
- [ ] 收入模式：**10% 平台抽成 + 可选保险附加费**，解释为什么不选订阅/广告

### Task 3（9 分） — ERD
- [ ] **Chen 符号**（矩形=实体，菱形=关系，椭圆=属性，双椭圆=多值属性，下划线=主键）
- [ ] **≥ 7 个实体**（本项目 = 9 个）
- [ ] **≥ 1 个关联实体**（本项目 = Booking + Review = 2 个）
- [ ] 所有 M:N 都拆成关联实体
- [ ] 基数（1:1, 1:N, M:N）全部标注
- [ ] 有 narrative + assumptions 段

### Task 4（9 分） — BPMN
- [ ] 价值链 Primary Activities **表格**（Inbound / Operations / Outbound / Marketing & Sales / Service）
- [ ] BPMN 图采用 **Database Interaction Layout**（Class 5-1 slide 20）
- [ ] **独立的 Database pool/lane**
- [ ] **≥ 10 个活动**（本项目 = 14 个）
- [ ] 每个涉及数据的活动都有 `data flow` 箭头连到 DB
- [ ] 至少 1 个 sub-process 符号 + 解释（本项目 = Dispute Handling + Payment Processing = 2 个）
- [ ] 有 narrative + assumptions 段

### Task 5（2 分） — 反思
- [ ] 伦理问题**专属 CampusShare**（物品照片+地址的可推断敏感信息风险），不能是通用"隐私问题"
- [ ] 有具体 **mitigation**
- [ ] 有 **limitations** 与 **future work**

### 格式（贯穿全文）
- [ ] Title Page 含业务名 + 所有组员
- [ ] 直接引用用 `"双引号"`（非中文「」或单引号）
- [ ] APA 引用格式
- [ ] 未加密 `.doc` 或 `.pdf`

---

## 8. 禁止事项（避免跑题或超范围）

- ❌ **不要**在报告正文里引入机器学习/推荐算法细节（可在 Task 5 future work 里提一句）
- ❌ **不要**在 BPMN 里展开支付网关内部逻辑（用 sub-process 封装）
- ❌ **不要**建模跨校或跨城业务（在假设里排除）
- ❌ **不要**把 Host 和 Renter 拆成两个 Student 实体
- ❌ **不要**在 ERD 里加"管理员 / 平台运营"实体（会偏离核心业务）
- ❌ **不要**把战略写成"我们既低价又高品质"（违反 Porter 的 stuck-in-the-middle 警告）
- ❌ **不要**超过 3300 词（硬性扣分阈值）
- ❌ **不要**引用无法核实的数字或虚构的 citation —— 每一个统计数据必须来自真实存在的文献，且数字与原文一致（见下方引用规则）

---

## 8b. 引用完整性规则（Citation Integrity — 强制执行）

> **每次向任何草稿文件写入新的统计数据、市场数字、百分比或外部结论时，必须立即用 `/verify-citations` 进行核实，或在写作前先搜索确认来源存在且数字正确。**

具体规则：
1. **每个数字必须有出处** —— 不能凭印象写"约 X 亿"，必须能追溯到具体报告或文章
2. **付费数据库（IBISWorld、Statista）的数字只有被公开二次来源确认后才能引用**
3. **不能伪造 DOI、URL、页码**
4. **引用格式**：APA 7（见 `references/references.md` 中的模板）
5. **引用后同步更新 `references/references.md`**
6. **核查 Skill**：运行 `/verify-citations [文件名]` 可自动扫描并修正一个草稿文件中的 citation 问题

---

## 9. 尚待用户提供的信息（执行中会逐步问）

- [ ] **组员姓名列表** —— 用于 Title Page 与演讲分工
- [ ] **目标大学**（若有）—— 是否锚定某一所大学为首发（如无则用 "a mid-sized US university"）
- [ ] **Class 5-1 Slide 20 截图 / 说明** —— Database Interaction Layout 的具体样式
- [ ] **被分配的 peer feedback 目标组** —— 演讲日老师会分配
- [ ] **老师指定的引用格式细节**（如 APA 6 vs APA 7）

---

## 10. 工作区文件导航

```
/Users/alfred/Desktop/MIDS 301 Final Project/
├── MIDS301 S26 Group Assignment (1).docx   作业原文（勿改）
├── AGENT.md                                  ⭐ 本文件
├── README.md                                 工作区说明
├── drafts/
│   ├── 01_executive_summary.md
│   ├── 02_strategy.md
│   ├── 03_information_design.md
│   ├── 04_business_execution.md
│   └── 05_critical_reflection.md
├── diagrams/
│   ├── ERD_spec.md
│   ├── BPMN_spec.md
│   └── value_chain_table.md
├── presentation/
│   └── slides_outline.md
├── peer_feedback/
│   └── investor_feedback_template.md
├── references/
│   └── references.md
└── final/
    ├── CampusShare_Storage_Report.docx   （最终合稿，尚未生成）
    └── CampusShare_Storage_Slides.pptx    （最终 PPT，尚未生成）
```

---

## 11. 迭代规则

- 本文件每次重大决策（如改战略、改客户、改实体）后**第一个**更新
- 更新时在文末 Changelog 加一行
- 每个 Task 的 draft 文件开头必须写 `> 本文件遵循 AGENT.md，最后同步日期：YYYY-MM-DD`

---

## Changelog

- **2026-04-15**：初稿建立。确认业务方向（CampusShare Storage）、战略（Focused Differentiation）、9 实体 ERD、14 活动 BPMN、3000 词分配。

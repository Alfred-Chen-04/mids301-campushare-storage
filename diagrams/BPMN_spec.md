# BPMN Specification — Booking & Item Handover Process

> 本文件遵循 [`../AGENT.md`](../AGENT.md)，最后同步日期：2026-04-15
>
> **标准**：BPMN 2.0
> **布局**：Database Interaction Layout（Class 5-1 slide 20）
> **作图工具**：draw.io（启用 BPMN shape library）
> **导出**：`BPMN.png`（横向 A4），嵌入报告 Task 4

## 满分校验

- [x] 活动数 = **14**（要求 ≥ 10）
- [x] 至少 1 个独立 Database pool/lane → **Pool: Database**
- [x] 每个数据活动都有 data flow 到 DB
- [x] Sub-process 数量 = **2**（Payment Processing, Dispute Handling）
- [ ] BPMN 2.0 符号合规（画图时检查）

---

## Pool / Lane 结构

```
╔══════════════════════════════════════════════════════════╗
║ Pool 1: CampusShare Platform                              ║
║ ┌────────────────────────────────────────────────────┐   ║
║ │ Lane: Renter                                       │   ║
║ └────────────────────────────────────────────────────┘   ║
║ ┌────────────────────────────────────────────────────┐   ║
║ │ Lane: Host                                         │   ║
║ └────────────────────────────────────────────────────┘   ║
║ ┌────────────────────────────────────────────────────┐   ║
║ │ Lane: System                                       │   ║
║ └────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════╝

╔══════════════════════════════════════════════════════════╗
║ Pool 2: Database (data store)                             ║
║ [Student] [StorageListing] [Booking] [StoredItem]        ║
║ [Payment] [Review]                                        ║
╚══════════════════════════════════════════════════════════╝

╔══════════════════════════════════════════════════════════╗
║ Pool 3: Payment Gateway (external system) — optional      ║
╚══════════════════════════════════════════════════════════╝
```

---

## 14 活动详细规范

| # | Lane | Activity Name | BPMN Type | DB Interaction | Notes |
|---|---|---|---|---|---|
| 1 | Renter | Log in & verify .edu email | Task | READ `Student` | Start event 之后第一步 |
| 2 | Renter | Enter search criteria (location, dates, size) | User Task | — | |
| 3 | System | Query available listings | Service Task | READ `StorageListing` | |
| 4 | Renter | Browse and select a listing | User Task | — | |
| 5 | Renter | Submit booking request | User Task | WRITE `Booking` (status=pending) | |
| 6 | Host | Receive notification | Receive Task | — | |
| 7 | Host | Approve or decline | User Task (**Gateway**) | UPDATE `Booking` | XOR gateway：approve → 继续；decline → End |
| 8 | Renter | Upload item photos & declare value | User Task | WRITE `StoredItem` | |
| 9 | Renter | Pay deposit + first period | **Sub-process: Payment Processing** | WRITE `Payment` | 封装 Stripe 交互 |
| 10 | System | Generate agreement | Service Task | UPDATE `Booking` (status=confirmed) | |
| 11 | Renter + Host | Arrange handover time | User Task (collaboration) | — | 两 lane 之间 message flow |
| 12 | System | Record handover completion | Service Task | UPDATE `Booking` (status=active) | 若异议 → **Sub-process: Dispute Handling** |
| 13 | System | End-of-term return confirmation | User Task | UPDATE `Booking` (status=completed) | |
| 14 | System | Prompt mutual review | Service Task | WRITE `Review` | End event 之后 |

---

## Sub-process 详情

### Sub-process A: Payment Processing（活动 9）
**内部步骤**（draw.io 可双击展开）：
- Validate payment method
- Call Stripe API
- Handle response (success / failure)
- Write to `Payment` table

**为什么用 sub-process**：封装第三方支付网关的技术细节，保持主流程的业务语义清晰。

### Sub-process B: Dispute Handling（活动 12 的异常分支）
**内部步骤**：
- Capture dispute reason
- Write to `Dispute` table
- Notify both parties
- Platform mediation
- Resolve or escalate

**为什么用 sub-process**：异常路径独立，避免污染主成功路径；Dispute 内部流程复杂度较高。

---

## Data Flow 箭头规则

- 每个 DB 读写活动都有一条 **虚线箭头**（BPMN association / data association）连到 Pool 2 的对应实体
- 方向：
  - READ：DB → Activity
  - WRITE / UPDATE：Activity → DB

---

## 画图建议（draw.io 操作步骤）

1. https://app.diagrams.net/ → New Diagram
2. Shapes panel → More Shapes → 勾选 "BPMN 2.0"
3. 先画 3 个 Pool（Horizontal Pool）
4. Pool 1 内部分 3 条 Lane（Renter / Host / System）
5. 按 14 活动表顺序放 Task 方块，用顺序流（实线箭头）连接
6. Gateway 用菱形（XOR exclusive gateway）
7. Sub-process 用带 `+` 号的圆角矩形
8. Data store 用"桶"图标放在 Pool 2
9. Data association 用虚线箭头
10. 导出：File → Export as → PNG（Border 20, 300 DPI, whole diagram）
11. 保存为 `../diagrams/BPMN.png`

---

## 待确认事项

- [ ] Class 5-1 Slide 20 的 "Database Interaction Layout" 是否要求 Database pool 在顶部/底部/右侧？（等用户确认课件）
- [ ] 是否需要显式画出 Payment Gateway 外部 pool？（可选，画了更完整但占空间）

# ERD Specification — Chen Notation

> 本文件遵循 [`../AGENT.md`](../AGENT.md)，最后同步日期：2026-04-15
>
> **作图工具**：draw.io（https://app.diagrams.net/）
> **符号**：Chen notation — 矩形 = 实体，菱形 = 关系，椭圆 = 属性，双椭圆 = 多值属性，下划线 = 主键
> **导出**：`ERD.png`（600 DPI 或 SVG），嵌入报告 Task 3

## 满分校验

- [x] 实体数 = **9**（要求 ≥ 7）
- [x] 关联实体数 = **2**（Booking, Review；要求 ≥ 1）
- [x] 所有 M:N 已拆解
- [ ] 基数标注（画图时检查）
- [ ] 所有属性都有椭圆（画图时检查）
- [ ] 主键下划线（画图时检查）

---

## 实体清单（9 个）

### 1. Student
- **student_id** (PK)
- edu_email
- name
- phone
- rating_avg （派生属性，可用双线椭圆）
- verified_flag
- created_at

### 2. University
- **univ_id** (PK)
- name
- country
- address
- email_domain （用于 .edu 验证，如 "berkeley.edu"）

### 3. StorageListing
- **listing_id** (PK)
- host_id (FK → Student)
- title
- address
- size_cuft （立方英尺）
- price_per_day
- available_from
- available_to
- description

### 4. Booking ⭐ 关联实体（连接 Student-as-Renter × StorageListing）
- **booking_id** (PK)
- renter_id (FK → Student)
- listing_id (FK → StorageListing)
- start_date
- end_date
- total_price
- status （pending / confirmed / active / completed / cancelled）
- created_at

### 5. Payment
- **payment_id** (PK)
- booking_id (FK)
- amount
- type （deposit / first_period / final / refund）
- method
- status
- timestamp

### 6. StoredItem
- **item_id** (PK)
- booking_id (FK)
- description
- photo_url （多值属性 → 双椭圆）
- declared_value

### 7. Review ⭐ 关联实体（连接 reviewer Student × reviewee Student via Booking）
- **review_id** (PK)
- booking_id (FK)
- reviewer_id (FK → Student)
- reviewee_id (FK → Student)
- rating （1–5）
- comment
- created_at

### 8. Dispute
- **dispute_id** (PK)
- booking_id (FK)
- raised_by (FK → Student)
- reason
- resolution
- status （open / resolved / escalated）

### 9. InsurancePolicy
- **policy_id** (PK)
- booking_id (FK)
- coverage_amount
- premium
- terms
- status

---

## 关系清单（Chen 菱形）

| 关系菱形 | 连接 | 基数 | 说明 |
|---|---|---|---|
| **belongs to** | Student — University | N:1 | 每个 Student 属于一所 University |
| **creates** | Student (Host) — StorageListing | 1:N | Host 可以发布多个房源 |
| **initiates** | Student (Renter) — Booking | 1:N | Renter 可以发起多个 Booking |
| **booked for** | Booking — StorageListing | N:1 | 一个 Booking 指向一个 Listing |
| **contains** | Booking — StoredItem | 1:N | 一个 Booking 可含多件物品 |
| **settles** | Booking — Payment | 1:N | 一个 Booking 可能多次支付 |
| **generates** | Booking — Review | 1:2 (optional) | 双方可各评一次 |
| **reviewer** | Student — Review | 1:N | 某学生是多个 Review 的撰写人 |
| **reviewee** | Student — Review | 1:N | 某学生被多个 Review 评价 |
| **raises** | Booking — Dispute | 1:0..N | 一个 Booking 可能出现 0+ 纠纷 |
| **covered by** | Booking — InsurancePolicy | 1:0..1 | 可选保险 |

---

## 画图建议（draw.io 操作步骤）

1. 打开 https://app.diagrams.net/
2. 新建 Blank Diagram
3. 左侧 Shape panel → More Shapes → 勾选 "Entity Relation (Chen)"
4. 按上表先画实体（矩形），再画关系（菱形）
5. 属性（椭圆）连接到实体
6. 关联实体 Booking 和 Review 的菱形内写关系名，并把菱形本身也当实体画（可标注 "associative entity"）
7. 基数标在连线上（"1" 或 "N"）
8. 画完：File → Export as → PNG（Border 20px, 300 DPI）
9. 保存为 `../diagrams/ERD.png`

## 布局建议（A4 横向一页内）

```
+----------+      +-----------------+      +---------+
|University|←----→|    Student      |←----→| Review  |
+----------+      +-----------------+      +---------+
                   ↓        ↓      ↑            ↑
           +---------+  +---------+              |
           |Listing  |  | Booking |←-------------+
           +---------+  +---------+
                           ↓    ↓     ↓
                    +--------+ +-----+ +--------+
                    |Payment | |Item | |Dispute |
                    +--------+ +-----+ +--------+
                                         |
                                    +----------+
                                    |Insurance |
                                    +----------+
```

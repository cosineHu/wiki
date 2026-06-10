# Reading Map: 电商领域项目蓝图大纲标准方案库

## 入口文档

**电商领域项目蓝图大纲_标准方案库**
→ `raw/2026-06-05-blueprint-standard-library/blueprint-standard-library.md`

## 四级树形结构

```
蓝图分类（6个）
├── 主数据
│   ├── 基础档案 → 基础资料/组织档案/对接流程
│   ├── 商品主数据 → 属性定义/物料类别/物料管理
│   └── 用户中心 → 权限管理/部门/岗位/员工/账户
├── 库存管理
│   ├── 采购管理 → 采购入库/退货
│   ├── 库存管理 → 调拨/调整/移仓/锁定
│   └── 库存同步 → 平台关联/同步架构
├── 电商订单管理
│   ├── B2C销售 → 订单履约/下载/促销/合并/适配/拆单/快递/免审/推送/出库/回写
│   ├── B2C售后 → 取消/退货退款/换货/包裹单
│   ├── B2B销售 → 唯品会JIT/京东自营
│   └── B2B售后 → 唯品会退供/京东退货
├── 全渠道业务
│   └── 全渠道 → 分单策略/网订店发/店订店发/店订仓发/售后
└── 财务对账
    ├── 对账规则 → 收入确认/账套/会计期间/账单类型/下载导入
    ├── B2C对账
    ├── B2B对账
    └── 财务报表
```

## 与 wiki 现有内容的关联

| 本文概念 | wiki 页面 |
|---------|----------|
| 蓝图标准方案库 | `entities/blueprint-standard-library.md` |
| 蓝图结构分析 | `concepts/blueprint-outline-structure.md` |
| 百胜交付方法论 | `entities/baisheng-delivery-methodology.md` |
| 调研大纲体系 | `concepts/baisheng-survey-outline-system.md` |

## 交付场景依赖

作为蓝图大纲标准方案库，支撑：
- 场景3: 生成蓝图大纲 → 标准方案库 + 调研日志 → 项目蓝图大纲
- 场景4: 生成测试用例 → 蓝图大纲 → 功能点清单 → 测试用例

## 阅读建议

1. 先读 `entities/blueprint-standard-library.md` 了解六大分类
2. 再读 `concepts/blueprint-outline-structure.md` 了解裁剪和应用方法
3. 对照 `concepts/baisheng-survey-outline-system.md` 理解调研→蓝图的映射关系

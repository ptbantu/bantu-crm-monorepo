# @bantu/db

统一的 Prisma 数据库包，整合了 `apps/sales` 和 `apps/visa` 的数据模型。

## 功能

- 统一的 Prisma Client 导出
- 合并的数据库 schema（PostgreSQL）
- 支持多租户 RBAC（来自 sales）
- 支持签证管理（来自 visa）

## 安装

```bash
cd packages/database
pnpm install
```

## 配置

1. 复制 `.env.example` 为 `.env`
2. 配置 `DATABASE_URL` 和 `DIRECT_URL`

```bash
cp .env.example .env
```

## 使用

### 生成 Prisma Client

```bash
pnpm db:generate
```

### 推送 schema 到数据库

```bash
pnpm db:push
```

### 创建迁移

```bash
pnpm db:migrate
```

### 打开 Prisma Studio

```bash
pnpm db:studio
```

## 在其他包中使用

在 `apps/sales` 或 `apps/visa` 的 `package.json` 中添加依赖：

```json
{
  "dependencies": {
    "@bantu/db": "workspace:*"
  }
}
```

然后在代码中导入：

```typescript
import { prisma } from '@bantu/db'

// 使用 Prisma Client
const users = await prisma.user.findMany()
```

## Schema 结构

### Sales 模块
- 多租户 & RBAC（Organization, User, Role, Permission）
- 线索管理（Lead）
- 客户管理（SalesCustomer）
- 商机管理（Opportunity，P1-P8 阶段）
- 产品管理（Product）
- 互动记录（Interaction）
- 审计日志（ActionLog）

### Visa 模块
- 签证客户（VisaCustomer）
- 签证记录（VisaRecord）
- 签证类型（VisaType）
- 提醒管理（Reminder）
- 系统日志（SystemLog）
- 邮件处理日志（EmailProcessLog）
- 应用设置（AppSettings）

## 注意事项

- 原 `apps/visa` 使用 SQLite，现已统一为 PostgreSQL
- Sales 的 `Customer` 模型重命名为 `SalesCustomer`，以避免与 Visa 的 `Customer` 冲突
- Visa 的 `Customer` 模型重命名为 `VisaCustomer`

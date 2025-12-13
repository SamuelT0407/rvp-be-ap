# 基础类型定义模板

## 概述

用于快速创建 `types.ts` 文件，定义业务相关的 TypeScript 类型。

---

## 模板

将以下代码复制到 `src/api/{module}/{business}/types.ts`，替换 `Xxx` 为实际业务名称。

```typescript
/**
 * {业务名}查询参数
 */
export interface XxxQuery extends PageQuery {
  /** 名称 */
  name?: string;
  /** 状态 */
  status?: string;
  // TODO: 添加更多查询字段
}

/**
 * {业务名}返回对象
 * 对应后端 VO
 */
export interface XxxVO extends BaseEntity {
  /** 主键ID */
  id: string | number;
  /** 名称 */
  name: string;
  /** 状态 (0正常 1停用) */
  status: string;
  /** 备注 */
  remark?: string;
  // TODO: 添加更多字段
}

/**
 * {业务名}表单对象
 * 用于新增和编辑
 */
export interface XxxForm {
  /** 主键ID (编辑时有值) */
  id?: string | number;
  /** 名称 */
  name: string;
  /** 状态 */
  status: string;
  /** 备注 */
  remark?: string;
  // TODO: 添加更多字段
}

/**
 * {业务名}详情返回对象
 * 如果获取详情时返回的结构与 VO 不同，使用此类型
 */
export interface XxxInfoVO {
  /** 主体数据 */
  xxx: XxxVO;
  /** 关联数据 (如有) */
  // relatedList?: YyyVO[];
}
```

---

## 全局类型说明

以下类型已在全局定义，可直接使用:

### PageQuery

```typescript
interface PageQuery {
  pageNum: number;
  pageSize: number;
}
```

### BaseEntity

```typescript
interface BaseEntity {
  createBy?: string;
  createTime?: string;
  updateBy?: string;
  updateTime?: string;
}
```

---

## 命名规范

| 类型用途 | 命名格式           | 示例                     |
| -------- | ------------------ | ------------------------ |
| 查询参数 | `{Business}Query`  | `UserQuery`, `RoleQuery` |
| 返回对象 | `{Business}VO`     | `UserVO`, `RoleVO`       |
| 表单对象 | `{Business}Form`   | `UserForm`, `RoleForm`   |
| 详情封装 | `{Business}InfoVO` | `UserInfoVO`             |

---

## 字段类型约定

| 后端类型             | 前端类型           | 说明              |
| -------------------- | ------------------ | ----------------- |
| Long / Integer       | `string \| number` | ID 类型保持灵活性 |
| String               | `string`           | -                 |
| Boolean              | `boolean`          | -                 |
| Date / LocalDateTime | `string`           | 前端统一用字符串  |
| List<T>              | `T[]`              | -                 |

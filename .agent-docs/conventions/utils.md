# 工具函数规范

## 何时抽取工具函数

### 黄金法则

```
如果相同的逻辑处理出现 ≥2 次，必须抽取为工具函数
```

### 触发条件

- 相同的数据转换逻辑
- 相同的格式化逻辑
- 相同的校验逻辑
- 相同的计算逻辑

---

## 存放位置

### 通用工具

**存放位置**: `src/utils/`

适用于:

- 与业务无关的纯函数
- 可被任何模块使用

```
src/utils/
├── request.ts          # HTTP 请求封装
├── ruoyi.ts            # RuoYi 工具函数
├── permission.ts       # 权限校验
├── validate.ts         # 表单验证
├── formatDateTime.ts   # 日期格式化
└── index.ts            # 统一导出
```

### 业务工具

**存放位置**: 业务模块目录下

适用于:

- 与特定业务耦合
- 仅在某个模块内使用

```
src/views/system/user/
├── index.vue
├── utils/
│   └── userHelper.ts
└── ...
```

---

## 工具函数编写规范

### 基本结构

```typescript
/**
 * 函数功能描述
 * @param param1 参数1说明
 * @param param2 参数2说明
 * @returns 返回值说明
 * @example
 * formatDateTime(new Date()) // => "2024-01-01 12:00:00"
 */
export function functionName(param1: string, param2?: number): string {
  // 实现逻辑
  return result;
}
```

### 命名规范

| 类型   | 命名格式                       | 示例                            |
| ------ | ------------------------------ | ------------------------------- |
| 格式化 | `format{What}`                 | `formatDateTime`, `formatMoney` |
| 解析   | `parse{What}`                  | `parseJSON`, `parseQuery`       |
| 校验   | `is{What}` / `validate{What}`  | `isEmail`, `validatePhone`      |
| 转换   | `{from}To{To}`                 | `arrayToTree`, `treeToArray`    |
| 获取   | `get{What}`                    | `getToken`, `getQueryParams`    |
| 设置   | `set{What}`                    | `setToken`                      |
| 计算   | `calc{What}` / `compute{What}` | `calcTotal`                     |

---

## 常用工具函数示例

### 日期格式化

```typescript
/**
 * 格式化日期时间
 * @param date 日期对象或时间戳
 * @param format 格式字符串，默认 "YYYY-MM-DD HH:mm:ss"
 */
export function formatDateTime(date: Date | number, format = 'YYYY-MM-DD HH:mm:ss'): string {
  // 使用 dayjs 或自行实现
}
```

### 数组转树

```typescript
/**
 * 将扁平数组转换为树形结构
 * @param list 扁平数组
 * @param parentId 父级ID字段名
 * @param idKey ID字段名
 */
export function arrayToTree<T extends Record<string, any>>(list: T[], parentId = 'parentId', idKey = 'id'): T[] {
  // 实现逻辑
}
```

### 防抖函数

```typescript
/**
 * 防抖函数
 * @param fn 需要防抖的函数
 * @param delay 延迟时间（毫秒）
 */
export function debounce<T extends (...args: any[]) => any>(fn: T, delay: number) {
  let timer: NodeJS.Timeout | null = null;
  return (...args: Parameters<T>) => {
    if (timer) clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}
```

---

## 导出规范

### 单个函数文件

```typescript
// src/utils/formatDateTime.ts
export function formatDateTime() { ... }
export function formatDate() { ... }
```

### 统一导出入口

```typescript
// src/utils/index.ts
export * from './formatDateTime';
export * from './validate';
export * from './permission';
```

### 使用

```typescript
import { formatDateTime, isEmail } from '@/utils';
```

---

## Checklist

抽取工具函数前确认:

- [ ] 该逻辑确实在 ≥2 处使用
- [ ] 函数职责单一
- [ ] 添加完整的 JSDoc 注释
- [ ] 确定是通用工具还是业务工具
- [ ] 考虑边界情况和错误处理

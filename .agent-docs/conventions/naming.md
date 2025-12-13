# 命名规范

## 文件与目录命名

| 类型                | 规范                          | 示例                              |
| ------------------- | ----------------------------- | --------------------------------- |
| **目录**            | kebab-case (短横线)           | `user-profile/`, `dict-data/`     |
| **Vue 页面入口**    | `index.vue`                   | `/views/system/user/index.vue`    |
| **Vue 子页面**      | kebab-case.vue                | `auth-role.vue`, `profile.vue`    |
| **Vue 组件**        | PascalCase.vue                | `RightToolbar.vue`, `DictTag.vue` |
| **TypeScript 文件** | camelCase.ts 或 kebab-case.ts | `types.ts`, `index.ts`            |
| **工具函数**        | camelCase.ts                  | `formatDateTime.ts`               |

---

## Vue 组件命名

### 组件名 (defineOptions)

```typescript
// 使用 PascalCase
<script setup lang="ts" name="UserProfile">
```

### Props

```typescript
// 使用 camelCase
defineProps<{
  userId: number;
  showAvatar: boolean;
}>();
```

### Emits

```typescript
// 使用 camelCase，事件名使用 kebab-case 风格的动词
defineEmits<{
  (e: 'update', value: string): void;
  (e: 'delete', id: number): void;
}>();
```

---

## TypeScript 类型命名

| 类型           | 规范       | 示例                 |
| -------------- | ---------- | -------------------- |
| **Interface**  | PascalCase | `UserVO`, `RoleForm` |
| **Type Alias** | PascalCase | `StatusType`         |
| **Enum**       | PascalCase | `UserStatus`         |
| **泛型参数**   | 单字母大写 | `T`, `K`, `V`        |

### 业务类型后缀约定

| 后缀     | 用途     | 示例         |
| -------- | -------- | ------------ |
| `Query`  | 查询参数 | `UserQuery`  |
| `VO`     | 返回对象 | `UserVO`     |
| `Form`   | 表单对象 | `UserForm`   |
| `InfoVO` | 详情封装 | `UserInfoVO` |

---

## API 函数命名

| 操作     | 前缀          | 示例                       |
| -------- | ------------- | -------------------------- |
| 查询列表 | `list`        | `listUser`, `listRole`     |
| 获取单个 | `get`         | `getUser`, `getRole`       |
| 新增     | `add`         | `addUser`, `addRole`       |
| 修改     | `update`      | `updateUser`, `updateRole` |
| 删除     | `del`         | `delUser`, `delRole`       |
| 状态变更 | `change`      | `changeUserStatus`         |
| 导出     | `export`      | `exportUser`               |
| 下载模板 | `...Template` | `userTemplate`             |

---

## 变量与函数命名

### 常用变量命名

| 变量                   | 用途           |
| ---------------------- | -------------- |
| `dataList` / `xxxList` | 表格数据列表   |
| `loading`              | 加载状态       |
| `total`                | 总记录数       |
| `queryParams`          | 查询参数       |
| `form`                 | 表单数据       |
| `rules`                | 表单验证规则   |
| `dialog`               | 弹窗状态       |
| `ids`                  | 选中的 ID 数组 |
| `single`               | 是否单选       |
| `multiple`             | 是否多选       |

### 常用函数命名

| 函数           | 用途          |
| -------------- | ------------- |
| `getList`      | 获取列表数据  |
| `handleQuery`  | 搜索          |
| `resetQuery`   | 重置搜索      |
| `handleAdd`    | 打开新增弹窗  |
| `handleUpdate` | 打开编辑弹窗  |
| `handleDelete` | 删除操作      |
| `submitForm`   | 提交表单      |
| `reset`        | 重置表单数据  |
| `cancel`       | 取消/关闭弹窗 |

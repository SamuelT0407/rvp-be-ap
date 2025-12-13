# 组件封装规范

## 何时抽取组件

### 黄金法则

```
如果相同的 Template 结构出现 ≥2 次，必须抽取为独立组件
```

### 触发条件

- 相同的 UI 结构在多处出现
- 相同的交互逻辑可复用
- 需要统一的样式/行为变更入口

---

## 组件分类与存放位置

### 全局组件

**存放位置**: `src/components/`

适用于:

- 跨多个业务模块使用
- 通用 UI 组件 (如 Pagination, RightToolbar)

```
src/components/
├── Pagination/
│   └── index.vue
├── RightToolbar/
│   └── index.vue
└── DictTag/
    └── index.vue
```

### 业务组件

**存放位置**: 与使用页面同级目录

适用于:

- 仅在某个业务模块内复用
- 与特定业务逻辑耦合

```
src/views/system/user/
├── index.vue
├── components/
│   └── UserForm.vue
└── profile.vue
```

---

## 组件编写规范

### 文件结构

```vue
<template>
  <!-- 组件模板 -->
</template>

<script setup lang="ts" name="ComponentName">
// Props 定义
interface Props {
  modelValue: string;
  disabled?: boolean;
}
const props = withDefaults(defineProps<Props>(), {
  disabled: false
});

// Emits 定义
const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void;
  (e: 'change', value: string): void;
}>();

// 组件逻辑
</script>

<style scoped>
/* 组件样式 (如有) */
</style>
```

### Props 规范

```typescript
// ✅ 推荐: 使用 interface + withDefaults
interface Props {
  /** 绑定值 */
  modelValue: string;
  /** 是否禁用 */
  disabled?: boolean;
  /** 占位文本 */
  placeholder?: string;
}

const props = withDefaults(defineProps<Props>(), {
  disabled: false,
  placeholder: '请输入'
});
```

### Emits 规范

```typescript
// ✅ 推荐: 类型化 emits
const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void;
  (e: 'change', value: string): void;
  (e: 'submit'): void;
}>();

// 使用
emit('update:modelValue', newValue);
```

### v-model 支持

```typescript
// 支持 v-model
const modelValue = defineModel<string>('modelValue');

// 或手动实现
const props = defineProps<{ modelValue: string }>();
const emit = defineEmits<{ (e: 'update:modelValue', v: string): void }>();

const innerValue = computed({
  get: () => props.modelValue,
  set: (v) => emit('update:modelValue', v)
});
```

---

## 组件命名规范

| 类型      | 命名规则                 | 示例                              |
| --------- | ------------------------ | --------------------------------- |
| 组件文件  | PascalCase.vue           | `UserForm.vue`                    |
| 组件 name | PascalCase               | `UserForm`                        |
| 使用时    | PascalCase 或 kebab-case | `<UserForm />` 或 `<user-form />` |

---

## Checklist

抽取组件前确认:

- [ ] 该结构确实在 ≥2 处使用
- [ ] 明确输入 (Props) 和输出 (Emits)
- [ ] 考虑是全局组件还是业务组件
- [ ] 添加完整的类型定义和注释

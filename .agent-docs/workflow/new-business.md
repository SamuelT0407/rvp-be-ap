# 创建全新业务模块

## 概述

当需要从零开始创建一个全新的业务模块时，按照以下严格顺序执行。

---

## 完整流程

### Phase 1: API 层搭建

#### Step 1.1: 创建目录

```
src/api/{business-name}/
├── types.ts    # 类型定义
└── index.ts    # API 函数
```

#### Step 1.2: 编写 types.ts

根据后端 VO/DTO 定义前端类型：

```typescript
/**
 * 查询参数
 */
export interface XxxQuery extends PageQuery {
  name?: string;
  status?: string;
}

/**
 * 返回对象 (对应后端 VO)
 */
export interface XxxVO extends BaseEntity {
  id: string | number;
  name: string;
  status: string;
  // ... 其他字段
}

/**
 * 表单对象
 */
export interface XxxForm {
  id?: string | number;
  name: string;
  status: string;
  // ... 其他字段
}
```

> 参考模板: [templates/base-types.md](../templates/base-types.md)

#### Step 1.3: 编写 index.ts

根据后端 Controller 编写 API 函数：

```typescript
import request from '@/utils/request';
import { XxxQuery, XxxVO, XxxForm } from './types';

// 查询列表
export const listXxx = (query: XxxQuery) => {
  return request({ url: '/xxx/list', method: 'get', params: query });
};

// 获取详情
export const getXxx = (id: string | number) => {
  return request({ url: '/xxx/' + id, method: 'get' });
};

// 新增
export const addXxx = (data: XxxForm) => {
  return request({ url: '/xxx', method: 'post', data });
};

// 修改
export const updateXxx = (data: XxxForm) => {
  return request({ url: '/xxx', method: 'put', data });
};

// 删除
export const delXxx = (id: string | number | Array<string | number>) => {
  return request({ url: '/xxx/' + id, method: 'delete' });
};

export default { listXxx, getXxx, addXxx, updateXxx, delXxx };
```

> 参考模板: [templates/base-api.md](../templates/base-api.md)

---

### Phase 2: Views 层搭建

#### Step 2.1: 创建目录

```
src/views/{business-name}/
├── index.vue       # 主页面 (列表)
└── [其他].vue      # 子页面 (可选)
```

#### Step 2.2: 复制基础模板

使用 [templates/base-page.md](../templates/base-page.md) 中的基础页面模板。

模板包含：

- **Filter**: 搜索表单区域
- **Buttons**: 新增/修改/删除按钮
- **Table**: 数据表格
- **Pagination**: 分页组件
- **Dialog**: 新增/编辑弹窗
- **Script**: 标准函数 (getList, handleQuery, handleAdd, handleUpdate, handleDelete, submitForm, reset, cancel)

#### Step 2.3: 根据业务定制

1. 替换 `Xxx` 为实际业务名称
2. 修改 `queryParams` 字段
3. 修改表格列 `columns`
4. 修改表单字段和验证规则 `rules`
5. 调整 API 导入路径

---

### Phase 3: 路由配置 (如需要)

如果是独立页面，需要在路由中注册：

- 通常通过后端菜单管理动态生成
- 或在 `src/router/` 中手动配置

---

## Checklist

- [ ] `src/api/{business}/types.ts` 已创建
- [ ] `src/api/{business}/index.ts` 已创建
- [ ] `src/views/{business}/index.vue` 已创建
- [ ] 基础模板已替换为实际业务内容
- [ ] API 导入路径正确
- [ ] 表单验证规则完整
- [ ] 页面可正常 CRUD 操作

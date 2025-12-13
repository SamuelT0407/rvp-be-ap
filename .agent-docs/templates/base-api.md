# 基础 API 文件模板

## 概述

用于快速创建 `index.ts` 文件，封装 CRUD 接口调用。

---

## 模板

将以下代码复制到 `src/api/{module}/{business}/index.ts`，替换 `Xxx` / `xxx` 为实际业务名称。

```typescript
import request from '@/utils/request';
import { AxiosPromise } from 'axios';
import { XxxQuery, XxxVO, XxxForm, XxxInfoVO } from './types';

/**
 * 查询{业务名}列表
 * @param query 查询参数
 */
export const listXxx = (query: XxxQuery): AxiosPromise<XxxVO[]> => {
  return request({
    url: '/api/xxx/list',
    method: 'get',
    params: query
  });
};

/**
 * 获取{业务名}详情
 * @param id 主键ID
 */
export const getXxx = (id: string | number): AxiosPromise<XxxInfoVO> => {
  return request({
    url: '/api/xxx/' + id,
    method: 'get'
  });
};

/**
 * 新增{业务名}
 * @param data 表单数据
 */
export const addXxx = (data: XxxForm) => {
  return request({
    url: '/api/xxx',
    method: 'post',
    data: data
  });
};

/**
 * 修改{业务名}
 * @param data 表单数据
 */
export const updateXxx = (data: XxxForm) => {
  return request({
    url: '/api/xxx',
    method: 'put',
    data: data
  });
};

/**
 * 删除{业务名}
 * @param id 主键ID (支持批量，传入逗号分隔的 ID 或数组)
 */
export const delXxx = (id: Array<string | number> | string | number) => {
  return request({
    url: '/api/xxx/' + id,
    method: 'delete'
  });
};

/**
 * 修改{业务名}状态
 * @param id 主键ID
 * @param status 状态值
 */
export const changeXxxStatus = (id: string | number, status: string) => {
  return request({
    url: '/api/xxx/changeStatus',
    method: 'put',
    data: { id, status }
  });
};

// 默认导出 (推荐在页面中使用)
export default {
  listXxx,
  getXxx,
  addXxx,
  updateXxx,
  delXxx,
  changeXxxStatus
};
```

---

## 函数命名规范

| 操作类型 | 命名格式                 | HTTP 方法 | 示例               |
| -------- | ------------------------ | --------- | ------------------ |
| 查询列表 | `list{Business}`         | GET       | `listUser`         |
| 获取单个 | `get{Business}`          | GET       | `getUser`          |
| 新增     | `add{Business}`          | POST      | `addUser`          |
| 修改     | `update{Business}`       | PUT       | `updateUser`       |
| 删除     | `del{Business}`          | DELETE    | `delUser`          |
| 状态变更 | `change{Business}Status` | PUT       | `changeUserStatus` |
| 导出     | `export{Business}`       | POST      | `exportUser`       |
| 下载模板 | `{Business}Template`     | GET       | `userTemplate`     |

---

## 使用方式

### 方式1: 默认导出 (推荐)

```typescript
import api from '@/api/system/user';

// 调用
const res = await api.listUser(queryParams);
await api.addUser(formData);
```

### 方式2: 具名导入

```typescript
import { listUser, addUser, updateUser } from '@/api/system/user';

// 调用
const res = await listUser(queryParams);
await addUser(formData);
```

---

## 特殊场景

### 文件上传

```typescript
export const uploadFile = (data: FormData) => {
  return request({
    url: '/api/xxx/upload',
    method: 'post',
    data: data,
    headers: { 'Content-Type': 'multipart/form-data' }
  });
};
```

### 下载文件

```typescript
// 在页面中使用 proxy?.download
proxy?.download('api/xxx/export', queryParams, `xxx_${Date.now()}.xlsx`);
```

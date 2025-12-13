# API 层结构规范

## 概述

所有与后端交互的 API 请求都集中在 `src/api/` 目录下，按业务模块组织。

---

## 目录结构

```
src/api/
├── system/                 # 系统管理模块
│   ├── user/
│   │   ├── types.ts       # 类型定义
│   │   └── index.ts       # API 函数
│   ├── role/
│   ├── menu/
│   └── ...
├── monitor/               # 监控模块
├── workflow/              # 工作流模块
├── login.ts               # 登录相关 (独立文件)
└── types.ts               # 全局公共类型
```

---

## types.ts 规范

### 文件位置

`src/api/{module}/{business}/types.ts`

### 内容结构

```typescript
import { RoleVO } from '@/api/system/role/types'; // 引用其他模块类型

/**
 * 查询参数类型
 * 继承全局 PageQuery
 */
export interface XxxQuery extends PageQuery {
  name?: string;
  status?: string;
  // 查询参数通常都是可选的
}

/**
 * 返回对象类型 (对应后端 VO)
 * 继承全局 BaseEntity (包含 createTime, updateTime 等)
 */
export interface XxxVO extends BaseEntity {
  id: string | number;
  name: string;
  status: string;
  remark?: string;
  // 关联对象
  roles?: RoleVO[];
}

/**
 * 表单类型 (用于新增/编辑)
 */
export interface XxxForm {
  id?: string | number; // 新增时为空
  name: string;
  status: string;
  remark?: string;
}

/**
 * 详情返回类型 (如果与 VO 不同)
 */
export interface XxxInfoVO {
  xxx: XxxVO;
  relatedList: YyyVO[];
}
```

### 命名约定

| 类型     | 命名格式           | 示例         |
| -------- | ------------------ | ------------ |
| 查询参数 | `{Business}Query`  | `UserQuery`  |
| 返回对象 | `{Business}VO`     | `UserVO`     |
| 表单对象 | `{Business}Form`   | `UserForm`   |
| 详情封装 | `{Business}InfoVO` | `UserInfoVO` |

---

## index.ts 规范

### 文件位置

`src/api/{module}/{business}/index.ts`

### 内容结构

```typescript
import request from '@/utils/request';
import { AxiosPromise } from 'axios';
import { XxxQuery, XxxVO, XxxForm, XxxInfoVO } from './types';

/**
 * 查询列表
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
 * 获取详情
 * @param id 主键ID
 */
export const getXxx = (id: string | number): AxiosPromise<XxxInfoVO> => {
  return request({
    url: '/api/xxx/' + id,
    method: 'get'
  });
};

/**
 * 新增
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
 * 修改
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
 * 删除
 * @param id 主键ID (支持批量)
 */
export const delXxx = (id: Array<string | number> | string | number) => {
  return request({
    url: '/api/xxx/' + id,
    method: 'delete'
  });
};

// 默认导出对象 (方便页面统一调用)
export default {
  listXxx,
  getXxx,
  addXxx,
  updateXxx,
  delXxx
};
```

### 函数命名约定

| 操作     | 命名格式                  | 示例               |
| -------- | ------------------------- | ------------------ |
| 查询列表 | `list{Business}`          | `listUser`         |
| 获取单个 | `get{Business}`           | `getUser`          |
| 新增     | `add{Business}`           | `addUser`          |
| 修改     | `update{Business}`        | `updateUser`       |
| 删除     | `del{Business}`           | `delUser`          |
| 状态变更 | `change{Business}{Field}` | `changeUserStatus` |

---

## 使用方式

### 在 Vue 页面中调用

```typescript
// 方式1: 默认导出 (推荐)
import api from '@/api/system/user';
const res = await api.listUser(queryParams);

// 方式2: 具名导入
import { listUser, getUser } from '@/api/system/user';
const res = await listUser(queryParams);
```

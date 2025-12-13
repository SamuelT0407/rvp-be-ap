# 项目目录结构

## 核心目录说明

```
src/
├── api/                    # API 请求层
│   ├── system/            # 系统管理模块
│   │   ├── user/
│   │   │   ├── types.ts   # 类型定义
│   │   │   └── index.ts   # API 函数
│   │   ├── role/
│   │   ├── menu/
│   │   └── ...
│   ├── monitor/           # 监控模块
│   ├── workflow/          # 工作流模块
│   ├── login.ts           # 登录相关
│   └── types.ts           # 公共类型
│
├── views/                  # 页面组件
│   ├── system/            # 系统管理页面
│   │   ├── user/
│   │   │   ├── index.vue  # 用户列表页
│   │   │   └── profile.vue # 用户详情页 (可选)
│   │   ├── role/
│   │   └── ...
│   ├── monitor/
│   ├── login.vue          # 登录页
│   └── index.vue          # 首页
│
├── components/             # 全局可复用组件
│   ├── Pagination/        # 分页组件
│   ├── RightToolbar/      # 右侧工具栏
│   ├── DictTag/           # 字典标签
│   └── ...
│
├── utils/                  # 工具函数
│   ├── request.ts         # Axios 封装
│   ├── ruoyi.ts           # RuoYi 工具函数
│   ├── permission.ts      # 权限校验
│   └── ...
│
├── types/                  # 全局类型定义
│   ├── global.d.ts        # 全局类型声明
│   └── ...
│
├── store/                  # Pinia 状态管理
│   ├── modules/
│   │   ├── user.ts        # 用户状态
│   │   ├── app.ts         # 应用状态
│   │   └── ...
│   └── index.ts
│
├── router/                 # 路由配置
│   └── index.ts
│
├── plugins/                # 插件配置
│   ├── modal.ts           # 弹窗插件
│   └── ...
│
├── directive/              # 自定义指令
│   ├── permission/        # 权限指令 v-has-permi
│   └── ...
│
├── layout/                 # 布局组件
│   ├── components/
│   └── index.vue
│
├── assets/                 # 静态资源
│   ├── images/
│   ├── styles/
│   └── icons/
│
└── App.vue                # 根组件
```

---

## 命名约定

### 文件夹命名

- 使用 **kebab-case** (短横线分隔)
- 例: `user-profile/`, `dict-data/`

### Vue 文件命名

- 页面入口: `index.vue`
- 子页面/功能页: 功能名 + `.vue` (如 `profile.vue`, `auth-role.vue`)

### 组件命名

- 使用 **PascalCase** (大驼峰)
- 例: `RightToolbar.vue`, `DictTag.vue`

---

## 新增业务快速指引

1. **API 层**: `src/api/{module}/{business}/` 创建 `types.ts` + `index.ts`
2. **页面层**: `src/views/{module}/{business}/` 创建 `index.vue`
3. **如有可复用组件**: 放入 `src/components/` 或页面同级目录
4. **如有工具函数**: 放入 `src/utils/`

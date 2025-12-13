# 页面结构规范

## 概述

标准的 Admin Panel 页面由以下几个核心区域组成，从上到下依次排列。

---

## 页面布局

```
┌─────────────────────────────────────────────────────────┐
│                    Filter (搜索区域)                     │
│  ┌─────────────────────────────────────────────────────┐│
│  │ 表单项1  表单项2  表单项3  [搜索] [重置]              ││
│  └─────────────────────────────────────────────────────┘│
├─────────────────────────────────────────────────────────┤
│ Buttons: [新增] [修改] [删除] [更多▼]        [工具栏图标] │
├─────────────────────────────────────────────────────────┤
│                                                         │
│                    Table (数据表格)                      │
│  ┌────┬────────┬────────┬────────┬────────┬──────────┐ │
│  │ ☑  │ 字段1  │ 字段2  │ 字段3  │ 状态   │   操作   │ │
│  ├────┼────────┼────────┼────────┼────────┼──────────┤ │
│  │    │        │        │        │        │ 编辑 删除│ │
│  └────┴────────┴────────┴────────┴────────┴──────────┘ │
│                                                         │
├─────────────────────────────────────────────────────────┤
│             Pagination (分页)  [<  1 2 3 ... >]         │
└─────────────────────────────────────────────────────────┘

                    Dialog (弹窗 - 覆盖层)
┌─────────────────────────────────────────────────────────┐
│  标题: 新增/修改                               [X]      │
├─────────────────────────────────────────────────────────┤
│  表单字段1: [____________]                              │
│  表单字段2: [____________]                              │
│  ...                                                    │
├─────────────────────────────────────────────────────────┤
│                              [取消]  [确定]             │
└─────────────────────────────────────────────────────────┘
```

---

## 各区域规范

### 1. Filter (搜索区域)

```vue
<el-card shadow="hover">
  <el-form ref="queryFormRef" :model="queryParams" :inline="true">
    <el-form-item label="名称" prop="name">
      <el-input v-model="queryParams.name" placeholder="请输入名称" clearable @keyup.enter="handleQuery" />
    </el-form-item>
    <!-- 更多搜索项 -->
    <el-form-item>
      <el-button type="primary" icon="Search" @click="handleQuery">搜索</el-button>
      <el-button icon="Refresh" @click="resetQuery">重置</el-button>
    </el-form-item>
  </el-form>
</el-card>
```

**要点**：

- 使用 `el-form` + `inline` 模式
- 每个输入框绑定 `@keyup.enter="handleQuery"` 支持回车搜索
- 搜索和重置按钮放在最后

### 2. Buttons (操作按钮区)

```vue
<template #header>
  <el-row :gutter="10">
    <el-col :span="1.5">
      <el-button v-has-permi="['xxx:add']" type="primary" plain icon="Plus" @click="handleAdd">新增</el-button>
    </el-col>
    <el-col :span="1.5">
      <el-button v-has-permi="['xxx:edit']" type="success" plain :disabled="single" icon="Edit" @click="handleUpdate">修改</el-button>
    </el-col>
    <el-col :span="1.5">
      <el-button v-has-permi="['xxx:remove']" type="danger" plain :disabled="multiple" icon="Delete" @click="handleDelete">删除</el-button>
    </el-col>
    <right-toolbar v-model:show-search="showSearch" :columns="columns" @query-table="getList" />
  </el-row>
</template>
```

**要点**：

- 使用 `v-has-permi` 指令控制权限
- 批量操作按钮使用 `:disabled="single/multiple"` 控制

### 3. Table (数据表格)

```vue
<el-table v-loading="loading" border :data="dataList" @selection-change="handleSelectionChange">
  <el-table-column type="selection" width="50" align="center" />
  <el-table-column label="名称" prop="name" :show-overflow-tooltip="true" />
  <el-table-column label="状态" align="center">
    <template #default="scope">
      <el-switch v-model="scope.row.status" active-value="0" inactive-value="1" @change="handleStatusChange(scope.row)" />
    </template>
  </el-table-column>
  <el-table-column label="操作" fixed="right" width="150">
    <template #default="scope">
      <el-button v-has-permi="['xxx:edit']" link type="primary" icon="Edit" @click="handleUpdate(scope.row)" />
      <el-button v-has-permi="['xxx:remove']" link type="primary" icon="Delete" @click="handleDelete(scope.row)" />
    </template>
  </el-table-column>
</el-table>
```

**要点**：

- 首列使用 `type="selection"` 支持多选
- 操作列使用 `fixed="right"` 固定在右侧
- 长文本使用 `:show-overflow-tooltip="true"`

### 4. Pagination (分页)

```vue
<pagination v-show="total > 0" v-model:page="queryParams.pageNum" v-model:limit="queryParams.pageSize" :total="total" @pagination="getList" />
```

### 5. Dialog (弹窗)

```vue
<el-dialog v-model="dialog.visible" :title="dialog.title" width="600px" append-to-body @close="closeDialog">
  <el-form ref="formRef" :model="form" :rules="rules" label-width="80px">
    <!-- 表单内容 -->
  </el-form>
  <template #footer>
    <el-button type="primary" @click="submitForm">确 定</el-button>
    <el-button @click="cancel">取 消</el-button>
  </template>
</el-dialog>
```

**要点**：

- 使用 `append-to-body` 避免层级问题
- 关闭时调用 `closeDialog` 重置表单

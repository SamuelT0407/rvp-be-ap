# 基础页面模板

## 概述

这是一个标准的 Admin Panel 列表页面模板，包含 Filter、Buttons、Table、Pagination、Dialog 五大区域。

---

## 完整模板

将以下代码复制到新建的 `index.vue`，然后替换 `Xxx` 为实际业务名称。

```vue
<template>
  <div class="p-2">
    <!-- 搜索区域 -->
    <el-card shadow="hover" class="mb-[10px]">
      <el-form ref="queryFormRef" :model="queryParams" :inline="true">
        <el-form-item label="名称" prop="name">
          <el-input v-model="queryParams.name" placeholder="请输入名称" clearable @keyup.enter="handleQuery" />
        </el-form-item>
        <el-form-item label="状态" prop="status">
          <el-select v-model="queryParams.status" placeholder="请选择状态" clearable>
            <el-option v-for="dict in sys_normal_disable" :key="dict.value" :label="dict.label" :value="dict.value" />
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" icon="Search" @click="handleQuery">搜索</el-button>
          <el-button icon="Refresh" @click="resetQuery">重置</el-button>
        </el-form-item>
      </el-form>
    </el-card>

    <!-- 表格区域 -->
    <el-card shadow="hover">
      <template #header>
        <el-row :gutter="10">
          <el-col :span="1.5">
            <el-button v-has-permi="['xxx:xxx:add']" type="primary" plain icon="Plus" @click="handleAdd">新增</el-button>
          </el-col>
          <el-col :span="1.5">
            <el-button v-has-permi="['xxx:xxx:edit']" type="success" plain :disabled="single" icon="Edit" @click="handleUpdate()">修改</el-button>
          </el-col>
          <el-col :span="1.5">
            <el-button v-has-permi="['xxx:xxx:remove']" type="danger" plain :disabled="multiple" icon="Delete" @click="handleDelete()"
              >删除</el-button
            >
          </el-col>
          <right-toolbar v-model:show-search="showSearch" :columns="columns" @query-table="getList" />
        </el-row>
      </template>

      <el-table v-loading="loading" border :data="dataList" @selection-change="handleSelectionChange">
        <el-table-column type="selection" width="50" align="center" />
        <el-table-column v-if="columns[0].visible" label="ID" align="center" prop="id" />
        <el-table-column v-if="columns[1].visible" label="名称" align="center" prop="name" :show-overflow-tooltip="true" />
        <el-table-column v-if="columns[2].visible" label="状态" align="center">
          <template #default="scope">
            <el-switch v-model="scope.row.status" active-value="0" inactive-value="1" @change="handleStatusChange(scope.row)" />
          </template>
        </el-table-column>
        <el-table-column v-if="columns[3].visible" label="创建时间" align="center" prop="createTime" width="160" />
        <el-table-column label="操作" fixed="right" width="150">
          <template #default="scope">
            <el-tooltip content="修改" placement="top">
              <el-button v-has-permi="['xxx:xxx:edit']" link type="primary" icon="Edit" @click="handleUpdate(scope.row)" />
            </el-tooltip>
            <el-tooltip content="删除" placement="top">
              <el-button v-has-permi="['xxx:xxx:remove']" link type="primary" icon="Delete" @click="handleDelete(scope.row)" />
            </el-tooltip>
          </template>
        </el-table-column>
      </el-table>

      <pagination v-show="total > 0" v-model:page="queryParams.pageNum" v-model:limit="queryParams.pageSize" :total="total" @pagination="getList" />
    </el-card>

    <!-- 新增/编辑弹窗 -->
    <el-dialog v-model="dialog.visible" :title="dialog.title" width="500px" append-to-body @close="closeDialog">
      <el-form ref="formRef" :model="form" :rules="rules" label-width="80px">
        <el-form-item label="名称" prop="name">
          <el-input v-model="form.name" placeholder="请输入名称" />
        </el-form-item>
        <el-form-item label="状态">
          <el-radio-group v-model="form.status">
            <el-radio v-for="dict in sys_normal_disable" :key="dict.value" :value="dict.value">{{ dict.label }}</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="备注" prop="remark">
          <el-input v-model="form.remark" type="textarea" placeholder="请输入备注" />
        </el-form-item>
      </el-form>
      <template #footer>
        <el-button type="primary" @click="submitForm">确 定</el-button>
        <el-button @click="cancel">取 消</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script setup lang="ts" name="XxxIndex">
import api from '@/api/xxx'; // TODO: 替换为实际路径
import { XxxForm, XxxQuery, XxxVO } from '@/api/xxx/types'; // TODO: 替换为实际路径
import { to } from 'await-to-js';

const { proxy } = getCurrentInstance() as ComponentInternalInstance;
const { sys_normal_disable } = toRefs<any>(proxy?.useDict('sys_normal_disable'));

// ==================== 状态变量 ====================
const dataList = ref<XxxVO[]>([]);
const loading = ref(true);
const showSearch = ref(true);
const ids = ref<Array<number | string>>([]);
const single = ref(true);
const multiple = ref(true);
const total = ref(0);

// 列显隐
const columns = ref<FieldOption[]>([
  { key: 0, label: 'ID', visible: true, children: [] },
  { key: 1, label: '名称', visible: true, children: [] },
  { key: 2, label: '状态', visible: true, children: [] },
  { key: 3, label: '创建时间', visible: true, children: [] }
]);

// Refs
const queryFormRef = ref<ElFormInstance>();
const formRef = ref<ElFormInstance>();

// 弹窗状态
const dialog = reactive<DialogOption>({
  visible: false,
  title: ''
});

// ==================== 表单初始化 ====================
const initFormData: XxxForm = {
  id: undefined,
  name: '',
  status: '0',
  remark: ''
};

const initData: PageData<XxxForm, XxxQuery> = {
  form: { ...initFormData },
  queryParams: {
    pageNum: 1,
    pageSize: 10,
    name: '',
    status: ''
  },
  rules: {
    name: [{ required: true, message: '名称不能为空', trigger: 'blur' }]
  }
};

const data = reactive<PageData<XxxForm, XxxQuery>>(initData);
const { queryParams, form, rules } = toRefs<PageData<XxxForm, XxxQuery>>(data);

// ==================== 核心函数 ====================

/** 查询列表 */
const getList = async () => {
  loading.value = true;
  const res = await api.listXxx(queryParams.value);
  loading.value = false;
  dataList.value = res.rows;
  total.value = res.total;
};

/** 搜索 */
const handleQuery = () => {
  queryParams.value.pageNum = 1;
  getList();
};

/** 重置 */
const resetQuery = () => {
  queryFormRef.value?.resetFields();
  queryParams.value.pageNum = 1;
  handleQuery();
};

/** 多选 */
const handleSelectionChange = (selection: XxxVO[]) => {
  ids.value = selection.map((item) => item.id);
  single.value = selection.length !== 1;
  multiple.value = !selection.length;
};

/** 新增 */
const handleAdd = () => {
  reset();
  dialog.visible = true;
  dialog.title = '新增';
};

/** 修改 */
const handleUpdate = async (row?: XxxVO) => {
  reset();
  const id = row?.id || ids.value[0];
  const { data } = await api.getXxx(id);
  dialog.visible = true;
  dialog.title = '修改';
  Object.assign(form.value, data);
};

/** 删除 */
const handleDelete = async (row?: XxxVO) => {
  const delIds = row?.id || ids.value;
  const [err] = await to(proxy?.$modal.confirm('是否确认删除？') as any);
  if (!err) {
    await api.delXxx(delIds);
    await getList();
    proxy?.$modal.msgSuccess('删除成功');
  }
};

/** 状态变更 */
const handleStatusChange = async (row: XxxVO) => {
  const text = row.status === '0' ? '启用' : '停用';
  try {
    await proxy?.$modal.confirm(`确认要${text}吗？`);
    // await api.changeXxxStatus(row.id, row.status);  // TODO: 如有状态变更接口
    proxy?.$modal.msgSuccess(`${text}成功`);
  } catch {
    row.status = row.status === '0' ? '1' : '0';
  }
};

/** 提交表单 */
const submitForm = () => {
  formRef.value?.validate(async (valid: boolean) => {
    if (valid) {
      if (form.value.id) {
        await api.updateXxx(form.value);
      } else {
        await api.addXxx(form.value);
      }
      proxy?.$modal.msgSuccess('操作成功');
      dialog.visible = false;
      await getList();
    }
  });
};

/** 重置表单 */
const reset = () => {
  form.value = { ...initFormData };
  formRef.value?.resetFields();
};

/** 取消 */
const cancel = () => {
  dialog.visible = false;
  reset();
};

/** 关闭弹窗 */
const closeDialog = () => {
  dialog.visible = false;
  reset();
};

// ==================== 初始化 ====================
onMounted(() => {
  getList();
});
</script>
```

---

## 使用说明

1. **全局替换** `Xxx` / `xxx` 为你的业务名称 (如 `User` / `user`)
2. 修改 **权限标识** `xxx:xxx:add` 为实际权限 (如 `system:user:add`)
3. 修改 **API 导入路径**
4. 根据实际字段修改 **queryParams**、**columns**、**form**、**rules**

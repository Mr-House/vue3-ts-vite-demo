<template>
  <div style="margin-bottom: 10px;margin-top: 30px;text-align: left;">
    <a-button type="primary" @click="addRow">添加表头</a-button>
  </div>
  <a-table
    :data-source="items"
    :columns="columns"
    bordered
    size="small"
    :rowClassName="index => (index % 2 === 1 ? 'table-striped' : null)"
    :pagination="false"
  >
    <template #field="{ record }">
      <a-select
        show-search
        v-model:value="record.field"
        :options="elements"
        style="width: 90%;"
        @blur="handleChange(record)"
      ></a-select>
    </template>
    <template #span="{ record }">
      <a-select show-search v-model:value="record.span" :options="options" style="width: 90%;"></a-select>
    </template>
    <template #content="{ record }">
      <a-input v-model:value="record.content"></a-input>
    </template>
    <template #operation="{ index }">
      <a-button type="primary" danger @click="delRow(index)">删除</a-button>
    </template>
  </a-table>
</template>

<script lang="ts" setup>
import { reactive, computed } from 'vue'
import { ColumnProps } from 'ant-design-vue/es/table/interface';


type Key = ColumnProps['key'];

interface DataType {
  key: Key
  field: string
  label: string
  span: number
  content: string
}

interface SelectType {
  value: number
  label: string
}

let items = reactive<DataType[]>([
  { key: 1, label: '产品名称', field: 'productName', span: 24, content: '抗氧化剂 RIANOX 245' },
  { key: 2, label: 'CAS登录号', field: 'account', span: 24, content: '36443-68-2' },
  { key: 3, label: '批号', field: 'batch', span: 24, content: 'sh' },
  { key: 4, label: '生产日期', field: 'productionDate', span: 12, content: 'May 05, 2021' },
  { key: 5, label: '校验日期', field: 'validateDate', span: 12, content: 'May 05, 2021' },
  { key: 6, label: '数量', field: 'amount', span: 24, content: '1000kg' }
]);

const columns = reactive([
  {
    title: "字段",
    dataIndex: "fieldName",
    key: "field",
    slots: { customRender: 'field' },
    align: 'center',
    width: 300
  },
  {
    title: "占位格数",
    dataIndex: "span",
    key: "span",
    slots: { customRender: 'span' },
    align: 'center',
    width: 300
  },
  {
    title: "内容",
    dataIndex: "content",
    key: "content",
    slots: { customRender: 'content' },
    align: 'center',
    width: 300
  },
  {
    title: "操作",
    dataIndex: "operation",
    key: "operation",
    slots: { customRender: 'operation' },
    align: 'center',
    width: 300
  },
]);

const options = reactive<SelectType[]>([
  { value: 12, label: '半行' },
  { value: 24, label: '整行' },
])

const count = computed(() => items.length + 1);

const elements = reactive([
  { label: '产品名称', value: 'productName' },
  { label: 'CAS登录号', value: 'account' },
  { label: '批号', value: 'batch' },
  { label: '生产日期', value: 'productionDate' },
  { label: '校验日期', value: 'validateDate' },
  { label: '数量', value: 'amount' }
])


const addRow = () => {
  items.push({
    key: count.value,
    field: '',
    label: '',
    span: 24,
    content: ''
  })
}
const delRow = (index: number) => {
  items.splice(index, 1)
}

const handleChange = (record: DataType) => {
  const option = elements.filter(el => el.value === record.field)[0]
  record.label = option ? option.label : ''
}

const getData = () => {
  return items
}
defineExpose({
  getData
})
</script>

<style lang="less">
</style>
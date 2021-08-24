<template>
  <a-form layout="inline" labelAlign="right" :labelCol="{ span: 8 }" :wrapperCol="{ span: 16 }">
    <a-form-item :label="element.label" v-for="element in elements" :key="element.field">
      <a-input></a-input>
    </a-form-item>
  </a-form>

  <div style="margin-bottom: 10px;margin-top: 30px;text-align: left;">
    <a-button type="primary" @click="addRow">添加检验项目</a-button>
  </div>
  <a-table
    :data-source="dataSource"
    :columns="columns"
    bordered
    size="small"
    :rowClassName="index => (index % 2 === 1 ? 'table-striped' : null)"
    :pagination="false"
  >
    <template #inspectionItem="{ record }">
      <a-select
        show-search
        v-model:value="record.inspectionItem"
        :options="options"
        style="width: 90%;"
        @blur="handleChange(record)"
      ></a-select>
    </template>
    <template #inspectionResult="{ record }">
      <a-input v-model:value="record.inspectionResult"></a-input>
    </template>
    <template #operation="{ index }">
      <a-button type="primary" danger @click="delRow(index)">删除</a-button>
    </template>
  </a-table>
  <div style="text-align:center;">
    <a-button type="primary">打印预览</a-button>
  </div>
</template>

<script lang="ts" setup>
import { reactive, computed } from 'vue'
import { ColumnProps } from 'ant-design-vue/es/table/interface';


type Key = ColumnProps['key'];

interface DataType {
  key: Key
  inspectionItem: string
  indicator: string
  inspectionResult: string
}

interface SelectType {
  value: string
  label: string,
  indicator: string
}

let dataSource = reactive<DataType[]>([]);

const columns = reactive([
  {
    title: "检验项目",
    dataIndex: "inspectionItem",
    key: "inspectionItem",
    slots: { customRender: 'inspectionItem' },
    align: 'center',
    width: 300
  },
  {
    title: "规格指标",
    dataIndex: "indicator",
    key: "indicator",
    align: 'center',
    width: 300
  },
  {
    title: "检验结果",
    dataIndex: "inspectionResult",
    key: "inspectionResult",
    slots: { customRender: 'inspectionResult' },
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
  { value: 'Appearance', label: '外观', indicator: '白色～类白色粉末' },
  { value: 'Melting point', label: '熔点', indicator: '77.0~81.0' },
  { value: 'Volatiles', label: '挥发分，%', indicator: '0.50max' },
  { value: 'Ash', label: '灰分，%', indicator: '0.10max' },
  { value: 'Transmittance', label: '透光率 425nm，%', indicator: '95.0min' },
  { value: 'Clarity of solution', label: '溶清性', indicator: '澄清透明' },
  { value: 'Assay', label: '含量，%', indicator: '96.0min' },
])

const count = computed(() => dataSource.length + 1);

const elements = reactive([
  { label: '产品名称', field: 'productName' },
  { label: 'CAS登录号', field: 'account' },
  { label: '批号', field: 'batch' },
  { label: '生产日期', field: 'productionDate' },
  { label: '校验日期', field: 'validateDate' },
  { label: '数量', field: 'amount' }
])


const addRow = () => {
  dataSource.push({
    key: count.value,
    inspectionItem: '',
    indicator: '',
    inspectionResult: ''
  })
}
const delRow = (index: number) => {
  dataSource.splice(index, 1)
}

const handleChange = (record: DataType) => {
  const option: SelectType = options.filter(el => el.value === record.inspectionItem)[0]
  record.indicator = option ? option.indicator : ''
}
</script>

<style>
</style>
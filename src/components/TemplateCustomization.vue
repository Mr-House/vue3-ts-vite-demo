<template>
  <a-form layout="inline">
    <a-row>
      <a-col v-for="element in elements" :span="element.span" :key="element.field" style="padding: 5px 0;">
        <a-form-item>
          <template #label>
            <span style="width: 100px;">{{element.label}}</span>
          </template>
          <a-input></a-input>
        </a-form-item>
      </a-col>
    </a-row>
  
  </a-form>

  <div style="margin-bottom: 10px;margin-top: 30px;text-align: left;">
    <a-button type="primary" @click="addRow">添加检验项目</a-button>
  </div>
  <a-table
    :data-source="items"
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
  <div style="text-align:center;margin-top: 20px;">
    <a-button type="primary" @click="save" style="margin-right: 5px;">保存</a-button>
    <a-button type="primary" @click="preview">预览</a-button>
  </div>
  <a-modal
    v-model:visible="visible"
    title="模版预览"
    width="100%"
    wrapClassName="full-modal"
    @ok="handleOk"
  >
    <Preview v-if="visible" :items="items" :elements="elements"/>
  </a-modal>
</template>

<script lang="ts" setup>
import { reactive, ref, computed, onMounted } from 'vue'
import { ColumnProps } from 'ant-design-vue/es/table/interface';
import Preview from './Preview.vue'


type Key = ColumnProps['key'];

interface DataType {
  key: Key
  inspectionItem: string
  inspectionItemName: string
  indicator: string
  inspectionResult: string
}

interface SelectType {
  value: string
  label: string,
  indicator: string
}

const visible = ref(false)

let items = reactive<DataType[]>([]);

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

const count = computed(() => items.length + 1);

const elements = reactive([
  { label: '产品名称', field: 'productName', span: 24 },
  { label: 'CAS登录号', field: 'account', span: 24 },
  { label: '批号', field: 'batch', span: 24 },
  { label: '生产日期', field: 'productionDate', span: 12 },
  { label: '校验日期', field: 'validateDate', span: 12 },
  { label: '数量', field: 'amount', span: 24 }
])


const addRow = () => {
  items.push({
    key: count.value,
    inspectionItem: '',
    inspectionItemName: '',
    indicator: '',
    inspectionResult: ''
  })
}
const delRow = (index: number) => {
  items.splice(index, 1)
}

const handleChange = (record: DataType) => {
  const option: SelectType = options.filter(el => el.value === record.inspectionItem)[0]
  record.indicator = option ? option.indicator : ''
  record.inspectionItemName = option ? option.label : ''
}

const preview = () => {
  visible.value = true
}

const handleOk = () => {
  visible.value = false
}

onMounted(() => {
  const data = localStorage.getItem('items')
  if (data) {
    JSON.parse(data).forEach(el => {
      items.push({...el})
    })
  }
})

const save = () =>  {
  localStorage.setItem('items', JSON.stringify(items))
}
</script>

<style lang="less">
.full-modal {
  .ant-modal {
    max-width: 100%;
    top: 0;
    padding-bottom: 0;
    margin: 0;
  }
  .ant-modal-content {
    display: flex;
    flex-direction: column;
    height: calc(100vh);
  }
  .ant-modal-body {
    flex: 1;
  }
}
</style>
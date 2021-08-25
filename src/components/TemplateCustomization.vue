<template>
  <Header ref="headerRef"></Header>
  <div style="padding-bottom: 10px;padding-top: 30px;text-align: left;">
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
  <div style="padding-top: 20px;">
    <a-form layout="inline">
      <a-form-item label="审核人">
        <a-input v-model:value="audit"></a-input>
      </a-form-item>
    </a-form>
  </div>
  <div style="text-align:center;padding-top: 20px;">
    <!-- <a-button type="primary" @click="save" style="margin-right: 5px;">保存</a-button> -->
    <a-button type="primary" @click="preview">预览</a-button>
  </div>
  <a-modal
    v-model:visible="visible"
    title="模版预览"
    width="100%"
    wrapClassName="full-modal"
    @ok="handleOk"
  >
    <Preview v-if="visible" :items="items" :elements="headerData" :audit="audit" />
  </a-modal>
</template>

<script lang="ts" setup>
import { reactive, ref, computed } from 'vue'
import { ColumnProps } from 'ant-design-vue/es/table/interface';
import Preview from './Preview.vue'
import Header from './Header.vue'


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

const audit = ref()

let items = reactive<DataType[]>([
  {
    key: 1,
    inspectionItem: 'Appearance',
    inspectionItemName: '外观',
    indicator: '白色～类白色粉末',
    inspectionResult: '白色粉末'
  },
  {
    key: 2,
    inspectionItem: 'Melting point',
    inspectionItemName: '熔点',
    indicator: '77.0~81.0',
    inspectionResult: '78.6~79.6'
  },
  {
    key: 3,
    inspectionItem: 'Volatiles',
    inspectionItemName: '挥发分，%',
    indicator: '0.50max',
    inspectionResult: '0.09'
  },
  {
    key: 4,
    inspectionItem: 'Ash',
    inspectionItemName: '灰分，%',
    indicator: '0.10max',
    inspectionResult: '0.01'
  },
  {
    key: 5,
    inspectionItem: 'Transmittance',
    inspectionItemName: '透光率 425nm，%',
    indicator: '95.0min',
    inspectionResult: '99.7'
  },
  {
    key: 6,
    inspectionItem: 'Clarity of solution',
    inspectionItemName: '溶清性',
    indicator: '澄清透明',
    inspectionResult: '澄清透明'
  },
  {
    key: 7,
    inspectionItem: 'Assay',
    inspectionItemName: '含量，%',
    indicator: '96.0min',
    inspectionResult: '99.1'
  },
]);

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


const headerRef = ref()

let headerData = ref([])
const preview = () => {
  headerData.value = headerRef.value.getData()
  visible.value = true
}

const handleOk = () => {
  visible.value = false
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
<template>
  <div>
    <div>
      <br />
      <h3>产品检验报告单</h3>
      <br />
      <h3>CERTIFICATE OF ANALYSIS</h3>
      <br />
    </div>
    <div>
      <a-row>
        <a-col :span="6">产品名称</a-col>
        <a-col :span="18">抗氧化剂 RIANOX 245</a-col>
      </a-row>
      <a-row>
        <a-col :span="6">CAS登录号</a-col>
        <a-col :span="18">36443-68-2</a-col>
      </a-row>
      <a-row>
        <a-col :span="6">批号</a-col>
        <a-col :span="18">1015105034</a-col>
      </a-row>
      <a-row>
        <a-col :span="6">生产日期</a-col>
        <a-col :span="6">May 05, 2021</a-col>
        <a-col :span="6">校验日期</a-col>
        <a-col :span="6">May 05, 2021</a-col>
      </a-row>
      <a-row>
        <a-col :span="6">数量</a-col>
        <a-col :span="18">1000kg</a-col>
      </a-row>
    </div>
    <div>
      <a-row>
        <a-col :span="8">检验项目</a-col>
        <a-col :span="8">规格指标</a-col>
        <a-col :span="8">检验结果</a-col>
      </a-row>
      <a-row v-for="item in items" :key="item.key">
        <a-col :span="8">{{ item.inspectionItem }}</a-col>
        <a-col :span="8">{{ item.indicator }}</a-col>
        <a-col :span="8">{{ item.inspectionResult }}</a-col>
      </a-row>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { ColumnProps } from 'ant-design-vue/es/table/interface';

const props = defineProps({
  items: {
    type: Array,
    default: (): DataType[] => ([])
  }
})

type Key = ColumnProps['key'];

interface DataType {
  key: Key
  inspectionItem: string
  indicator: string
  inspectionResult: string
}
</script>
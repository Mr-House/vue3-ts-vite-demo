<template>
  <div style="margin: 0 300px;border-top: 1px solid;border-left: 1px solid;border-right: 1px solid;">
    <div style="border-bottom: 1px solid;">
      <br />
      <h3 style="text-align:center;font-size: 900;">产品检验报告单</h3>
      <br />
      <h3 style="text-align:center;font-size: 900;">CERTIFICATE OF ANALYSIS</h3>
      <br />
    </div>
    <div>
      <a-row style="border-bottom: 1px solid;">
        <a-col :span="6" style="text-align:center;border-right: 1px solid;">产品名称</a-col>
        <a-col :span="18" style="text-align:center;">抗氧化剂 RIANOX 245</a-col>
      </a-row>
      <a-row style="border-bottom: 1px solid;">
        <a-col :span="6" style="text-align:center;border-right: 1px solid;">CAS登录号</a-col>
        <a-col :span="18" style="text-align:center;">36443-68-2</a-col>
      </a-row>
      <a-row style="border-bottom: 1px solid;">
        <a-col :span="6" style="text-align:center;border-right: 1px solid;">批号</a-col>
        <a-col :span="18" style="text-align:center;">1015105034</a-col>
      </a-row>
      <a-row style="border-bottom: 1px solid;">
        <a-col :span="6" style="text-align:center;border-right: 1px solid;">生产日期</a-col>
        <a-col :span="6" style="text-align:center;border-right: 1px solid;">May 05, 2021</a-col>
        <a-col :span="6" style="text-align:center;border-right: 1px solid;">校验日期</a-col>
        <a-col :span="6" style="text-align:center;">May 05, 2021</a-col>
      </a-row>
      <a-row style="border-bottom: 1px solid;">
        <a-col :span="6" style="text-align:center;border-right: 1px solid;">数量</a-col>
        <a-col :span="18" style="text-align:center;">1000kg</a-col>
      </a-row>
    </div>
    <div>
      <a-row style="border-bottom: 1px solid;">
        <a-col :span="8" style="text-align:center;border-right: 1px solid;">检验项目</a-col>
        <a-col :span="8" style="text-align:center;border-right: 1px solid;">规格指标</a-col>
        <a-col :span="8" style="text-align:center;">检验结果</a-col>
      </a-row>
      <a-row v-for="item in items" :key="item.key" style="border-bottom: 1px solid;">
        <a-col :span="8" style="text-align:center;border-right: 1px solid;">{{ item.inspectionItemName }}</a-col>
        <a-col :span="8" style="text-align:center;border-right: 1px solid;">{{ item.indicator }}</a-col>
        <a-col :span="8" style="text-align:center;">{{ item.inspectionResult }}</a-col>
      </a-row>
    </div>
    <div>
      <a-row></a-row>
      <a-row></a-row>
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
<style scoped>

</style>
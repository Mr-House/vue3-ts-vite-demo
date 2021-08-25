<template>
  <div
    style="margin: 0 600px;border-top: 1px solid;border-left: 1px solid;border-right: 1px solid;"
  >
    <div style="border-bottom: 1px solid;">
      <br />
      <div style="border-bottom: 1px solid #ded9d9;"></div>
      <h3 style="text-align:center;">产品检验报告单</h3>
      <div style="border-bottom: 1px solid #ded9d9;"></div>
      <br />
      <div style="border-bottom: 1px solid #ded9d9;"></div>
      <h3 style="text-align:center;">CERTIFICATE OF ANALYSIS</h3>
      <div style="border-bottom: 1px solid #ded9d9;"></div>
      <br />
    </div>
    <div>
      <a-row style="border-bottom: 1px solid;">
        <a-col
          v-for="(element: any, index) in elements"
          :span="element.span"
          :style="{ 'border-bottom': index !== elements.length - 1 ? '1px solid' : '', display: 'flex' }"
        >
          <div
            :style="{ width: element.span === 24 ? '25%' : '50%', borderRight: '1px solid', paddingLeft: '10px' }"
          >
            <span>{{ element.label }}</span>
          </div>
          <div
            :style="{ width: element.span === 24 ? '75%' : '50%', borderRight: element.span === 12 && element.field !== 'validateDate' ? '1px solid' : '', paddingLeft: '10px' }"
          >
            <span>{{ element.content }}</span>
          </div>
        </a-col>
      </a-row>
    </div>
    <div>
      <a-row style="border-bottom: 1px solid;">
        <a-col :span="8" style="text-align:center;border-right: 1px solid;">
          <span>检验项目</span>
        </a-col>
        <a-col :span="8" style="text-align:center;border-right: 1px solid;">
          <span>规格指标</span>
        </a-col>
        <a-col :span="8" style="text-align:center;">
          <span>检验结果</span>
        </a-col>
      </a-row>
      <a-row v-for="(item: any) in items" :key="item.key" style="border-bottom: 1px solid;">
        <a-col :span="8" style="text-align:center;border-right: 1px solid;">
          <span>{{ item.inspectionItemName }}</span>
        </a-col>
        <a-col :span="8" style="text-align:center;border-right: 1px solid;">
          <span>{{ item.indicator }}</span>
        </a-col>
        <a-col :span="8" style="text-align:center;">
          <span>{{ item.inspectionResult }}</span>
        </a-col>
      </a-row>
    </div>
    <div>
      <a-row style="border-bottom: 1px solid;line-height: 30px;">
        <a-col :span="4" style="text-align:center;border-right: 1px solid;">
          <span style="font-size: 16px;">结论</span>
        </a-col>
        <a-col :span="20" style="text-align:center;">
          <span
            style="font-size:24px;"
          >合&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;格</span>
        </a-col>
      </a-row>
      <a-row style="border-bottom: 1px solid;line-height: 30px;">
        <a-col :span="4" style="text-align:center;border-right: 1px solid;">
          <span style="font-size: 16px;">备注</span>
        </a-col>
        <a-col :span="20" style="text-align:center;">
          <span></span>
        </a-col>
      </a-row>
      <a-row>
        <a-col :span="4" style="text-align:center;">
          <span>制表：检05</span>
        </a-col>
        <a-col :span="12" style="text-align:center;">
          <span></span>
        </a-col>
        <a-col :span="8" style="text-align:center;">
          <span>审核：{{ audit }}</span>
        </a-col>
      </a-row>
      <a-row>
        <a-col :span="4" style="text-align:center;">
          <span>Analyst:</span>
        </a-col>
        <a-col :span="12" style="text-align:center;">
          <span></span>
        </a-col>
        <a-col :span="8" style="text-align:center;">
          <span>Chief－inspector：</span>
        </a-col>
      </a-row>
    </div>
    <div style="padding-top: 30px;font-size: 10px;border-bottom: 1px solid">
      <div style="float:left">地址：天津市华苑产业区开华道20号F座20层</div>
      <div style="float:right">
        <div>电话：022—83718817</div>
        <div>传真：022—83718815</div>
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { ColumnProps } from 'ant-design-vue/es/table/interface';

const props = defineProps({
  items: {
    type: Array,
    default: (): DataType[] => ([])
  },
  elements: {
    type: Array,
    default: () => ([])
  },
  audit: {
    type: String,
    default: ''
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
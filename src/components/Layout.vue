<template>
  <a-layout
    class="layout-custom-trigger"
    :style="{ overflow: 'auto', height: '100vh', width: '100%', position: 'fixed', left: 0 }"
  >
    <a-layout-sider v-model:collapsed="collapsed" :trigger="null" collapsible>
      <div class="logo"></div>
      <a-menu theme="dark" mode="inline" v-model:selectedKeys="selectedKeys">
        <a-menu-item key="1">
          <user-outlined />
          <span>创建模版</span>
        </a-menu-item>
      </a-menu>
    </a-layout-sider>
    <a-layout :style="{ width: '100%', height: '100%', overflow: 'auto' }">
      <a-layout-header :style="{ background: '#fff', padding: 0 }">
        <menu-unfold-outlined
          v-if="collapsed"
          class="trigger"
          @click="() => (collapsed = !collapsed)"
        />
        <menu-fold-outlined v-else class="trigger" @click="() => (collapsed = !collapsed)" />
      </a-layout-header>
      <a-layout-content
        :style="{ margin: '24px 16px', padding: '24px', background: '#fff', minHeight: 'max-content' }"
      >
        <div style="padding: 20px;height: max-content;">
          <TemplateCustomization />
        </div>
      </a-layout-content>
    </a-layout>
  </a-layout>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'
import TemplateCustomization from './TemplateCustomization.vue';
import {
  UserOutlined,
  MenuUnfoldOutlined,
  MenuFoldOutlined,
} from '@ant-design/icons-vue';

const collapsed = ref(false)

let selectedKeys = reactive(['1'])

</script>

<style>
.layout-custom-trigger .trigger {
  font-size: 18px;
  line-height: 64px;
  padding: 0 24px;
  cursor: pointer;
  transition: color 0.3s;
}

.layout-custom-trigger .trigger:hover {
  color: #1890ff;
}

.layout-custom-trigger .logo {
  height: 32px;
  background: rgba(255, 255, 255, 0.3);
  margin: 16px;
}

.site-layout .site-layout-background {
  background: #fff;
}
</style>
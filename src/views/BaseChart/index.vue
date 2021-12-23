<template>
  <div class="base-chart">
    <div class="base-chart__render">
      <ChartRender :options="options" />
    </div>

    <ConfigPanel @change="handleRefreshOptions" />
  </div>
</template>

<script setup lang="ts">
import { ref, watch, reactive } from 'vue'
import ChartRender from '@/components/ChartRender/index.vue'
import ConfigPanel from './components/ConfigPanel.vue'
import { EChartsOption } from 'echarts'

let options = reactive({})

const handleRefreshOptions = (opt: EChartsOption) => {
  options.value = opt
  console.log(opt, options)
}
</script>

<style lang="scss">
.base-chart {
  position: fixed;
  padding: 20px 10px;
  display: flex;
  height: 100%;
  width: 65%;
  box-sizing: border-box;
  align-items: center;
  justify-content: center;

  &__render {
    width: 100%;
    height: 600px;
    // transform: scale(0.7);
  }
}
</style>

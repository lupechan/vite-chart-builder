<template>
  <div id="echart-render" />
</template>

<script setup lang="ts">
import { ref, defineProps, onMounted, watch } from 'vue'
import * as echarts from 'echarts'
import { EChartsOption } from 'echarts'

const props = defineProps({
  title: String,
  options: {
    type: Object as EChartsOption,
    required: true,
    default: () => ({})
  }
})

onMounted(() => {
  const charts = echarts.init(document.getElementById('echart-render'), null, { renderer: 'canvas' })
  if (!charts) {
    console.error('获取节点失败')
    return
  }

  watch(
    () => props.options,
    val => charts.setOption(val.value || {}),
    { deep: true, immediate: true }
  )
})

</script>

<style lang="scss">
#echart-render {
  width: 100%;
  height: 100%;
  min-height: 300px;
  min-width: 300px;
}
</style>

<template>
  <div class="config-wrapper">
    <div class="config-panel">
      <el-form label-width="80px">
        <el-collapse v-model="activeNames" @change="handleChange">
          <el-collapse-item title="标题设置" name="title">
            <el-form-item label="标题文字">
              <el-input v-model="title" />
            </el-form-item>

            <el-form-item label="字号">
              <el-input-number v-model="titleFontSize" :min="10" :max="32" />
            </el-form-item>

          </el-collapse-item>

          <el-collapse-item title="坐标轴设置" name="title">
            <el-form-item label="标题文字">
              <el-input v-model="title" />
            </el-form-item>

            <el-form-item label="字号">
              <el-input-number v-model="axisFontSize" :min="10" :max="32" />
            </el-form-item>

          </el-collapse-item>
        </el-collapse>
      </el-form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { EChartsOption } from 'echarts'
import { computed, reactive, ref, watch, watchEffect } from 'vue'

const activeNames = ref(['title'])
const isShowDrawer = ref(true)

const title = ref('长沙市气温降水量')
const colors = ['#5470C6', '#EE6666']
const titleFontSize = ref(24)
const axisFontSize = ref(20)

const options: EChartsOption = computed(() => ({
  title: {
    text: title,
    top: 0,
    padding: [20, 0, 0, 0],
    left: 'center',
    textStyle: {
      fontSize: titleFontSize
    }
  },
  grid: {
    top: 120
  },
  colors,
  tooltip: {
    trigger: 'axis',
    axisPointer: {
      type: 'cross',
    }
  },
  toolbox: {
    feature: {
      dataView: { show: true, readOnly: false },
      // restore: { show: true },
      saveAsImage: { show: true },
    },
    top: 'bottom'
  },
  legend: {
    data: ['Evaporation', 'Precipitation', 'Temperature'],
  },
  nameTextStyle: {
    fontSize: axisFontSize,
  },
  xAxis: [
    {
      type: 'category',
      axisTick: {
        alignWithLabel: true,
      },
      // prettier-ignore
      data: ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12(月)'],
      axisLabel: {
        fontSize: axisFontSize,
      },
    },
  ],
  yAxis: [
    {
      type: 'value',
      name: '降水量(mm)',
      min: 0,
      max: 500,
      position: 'right',
      axisLine: {
        show: true,
        lineStyle: {
          color: colors[0],
        },
      },
      axisLabel: {
        fontSize: axisFontSize,
        formatter: '{value}',
      },
    },
    {
      type: 'value',
      name: '温度 （°C）',
      min: -20,
      max: 30,
      position: 'left',
      axisLine: {
        show: true,
        lineStyle: {
          color: colors[1],
        },
      },
      axisLabel: {
        fontSize: axisFontSize,
      },
      // axisLabel: {
      //   formatter: "{value} （°C）"
      // }
    },
  ],
  series: [
    {
      name: '降水量（mm）',
      type: 'bar',
      data: [
        147.8, 135.3, 168.8, 106.8, 194.0, 181.7, 162.2, 68.6, 195.6, 80.1,
        24.8, 26.9,
      ],
    },
    {
      name: '温度',
      type: 'line',
      yAxisIndex: 1,
      // prettier-ignore
      data: [5.1, 10.0, 13.3, 17.0, 22.9, 26.3, 27.6, 29.1, 22.0, 16.3, 13.6, 6.1],
    },
  ]
}))

const emits = defineEmits(['change'])

const handleChange = name => {
  activeNames.value = name
}

watch(
  () => options,
  val => {
    return emits('change', val.value)
  },
  { deep: true, immediate: true  }
)

</script>

<style lang="scss">
.config-wrapper {
  position: fixed;
  width: 30%;
  height: 100%;
  padding: 20px;
  top: 0;
  right: 0;
  box-shadow: -8px 0px 8px 2px rgb(0 0 0 / 5%);
  background-color: white;

  .el-form-item__content {
    text-align: left;
  }
  // border-left: 1px solid #ddd;
//   overflow: visible;

//   &::after {
//     content: '设置';
//     font-size: 16px;
//     position: absolute;
//     right: calc(100% - 1px);
//     top: 50%;
//     display: flex;
//     align-items: center;
//     width: 30px;
//     height: 100px;
//     background-color: inherit;
//     box-shadow: 0 8px 10px -8px rgb(0 0 0 / 20%), -8px 0px 10px 2px rgb(0 0 0 / 10%);
//     cursor: pointer;
//     transform: translateY(-50%);
//     border-radius: 4px 0 0 4px;
//     border-left: inherit;
//   }
}

// .config-wrapper-modal {
//   display: block !important;
//   pointer-events: none;

//   .el-drawer.rtl {
//     left: 100%;
//   }
// }
</style>
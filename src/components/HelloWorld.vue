<script setup lang="ts">
import { WordCloud } from '@antv/g2plot'
import { onMounted, ref } from 'vue'


const wordCloudRef = ref<HTMLDivElement | null>(null)
onMounted(() => {
  if (wordCloudRef.value) {
    fetch('https://gw.alipayobjects.com/os/antvdemo/assets/data/antv-keywords.json')
      .then((res) => res.json())
      .then((data) => {
        const wordCloud = new WordCloud('wordCloudRef', {
          data,
          wordField: 'name',
          weightField: 'value',
          colorField: 'name',
          wordStyle: {
            fontFamily: 'Verdana',
            fontSize: [ 8, 32 ],
            rotation: 0,
          },
          // 返回值设置成一个 [0, 1) 区间内的值，
          // 可以让每次渲染的位置相同（前提是每次的宽高一致）。
          random: () => 0.5,
        })
        
        wordCloud.render()
      })
    
  }
})
</script>

<template>
  
  <div ref="wordCloudRef" id="wordCloudRef" class="word-cloud" />
</template>

<style scoped>
.word-cloud {
  width: 100%;
  height: 100%;
  
  canvas {
    cursor: inherit !important;
  }
  
  .g2-tooltip-value {
    margin-left: 6px !important;
  }
}

html.dark {
  .word-cloud {
    .g2-tooltip {
      color: rgb(166, 166, 166) !important;
      background-color: rgb(31, 31, 31) !important;
      box-shadow: rgba(0, 0, 0, 0.5) 0 2px 4px !important;
    }
  }
}
</style>

<template>
  <j-page-container>
    <div>
      {{ route.name }}
    </div>
    <j-permission-button
      type="primary"
      hasPermission="system/Relationship:add"
      @click="() => { status = status == 1 ? 0 : 1 }"
    >
      <template #icon>
        
      </template>

    </j-permission-button>

    <permission-button
      type="primary"
      :tooltip="{
        title: '测试'
      }"
    >

      你好 {{ status }}
    </permission-button>
    <br/>

  </j-page-container>
  
  <div style="width: 100%; height: 300px">
    <init-Amap
      @dragstart="mapClick"
      mapStyle="dark"
    >
      <el-amap-marker
        :position="[0,0]"
      />
    </init-Amap>
  </div>
</template>

<script setup lang="ts">
import { useRoute } from 'vue-router'
const route = useRoute()

const status = ref(1)

const mapClick = (e: any) => {
  console.log(e)
}
</script>

<style scoped>

</style>

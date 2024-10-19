<script setup lang="ts">

import { Graph } from '@antv/x6'
import {onMounted, ref} from "vue";
import {Stencil} from "@antv/x6-plugin-stencil";
const data = ref({
  nodes: [
    {
      id: 'node1',
      shape: 'rect',
      x: 40,
      y: 40,
      width: 100,
      height: 40,
      label: 'hello',
      attrs: {
        body: {
          stroke: '#8f8f8f',
          strokeWidth: 1,
          fill: '#fff',
          rx: 6,
          ry: 6,
        },
      },
    },
    {
      id: 'node2',
      shape: 'rect',
      x: 160,
      y: 180,
      width: 100,
      height: 40,
      label: 'world',
      attrs: {
        body: {
          stroke: '#8f8f8f',
          strokeWidth: 1,
          fill: '#fff',
          rx: 6,
          ry: 6,
        },
      },
    },
  ],
  edges: [
    {
      shape: 'edge',
      source: 'node1',
      target: 'node2',
      label: 'x6',
      attrs: {
        line: {
          stroke: '#8f8f8f',
          strokeWidth: 1,
        },
      },
    },
  ],
})

// 画布
const graph = ref()
// 模板
const stencil = ref()
onMounted(() => {
  console.log('mounted')
  graph.value = new Graph({
    container: document.getElementById('container')!,
    background: {
      color: '#F2F7FA',
    },
    // autoResize: true,
    grid: {
      visible: true,
      type: 'doubleMesh',
      args: [
        {
          color: '#eee', // 主网格线颜色
          thickness: 1, // 主网格线宽度
        },
        {
          color: '#ddd', // 次网格线颜色
          thickness: 1, // 次网格线宽度
          factor: 4, // 主次网格线间隔
        },
      ],
    },
  })
  graph.value.fromJSON(data.value)
  graph.value.centerContent()
  stencil.value = new Stencil({
    title: '流程节点',
    target: graph.value,
    stencilGraphWidth: 200,
    stencilGraphHeight: 180,
    groups: [
      {
        name: 'group1',
      },
      {
        name: 'group2',
      },
    ],
  })
  document.getElementById('stencil')!.appendChild(stencil.value.container)
  const rect1 = graph.value.createNode({
    shape: 'rect',
    width: 100,
    height: 40,
  })
  const rect2 = graph.value.createNode({
    shape: 'rect',
    width: 100,
    height: 40,
  })
  stencil.value.load([rect1, rect2], 'group1')
})
</script>

<template>
  <div id="workflow-designer">
    <div id="stencil"></div>
    <div id="container"></div>
  </div>
</template>

<style scoped lang="less">
#workflow-designer {
  display: flex;
  height: 100%;
  width: 100%;
  border: 1px solid #dfe3e8;
}
#stencil {
  width: 400px;
  height: 100%;
  position: relative;
  border-right: 1px solid #dfe3e8;
}
#container {
  width: calc(100% - 400px);
  height: 100%;
}

//#workflow-designer {
//  display: flex;
//  padding: 0;
//
//  #stencil {
//    position: relative;
//    width: 200px;
//    border: 1px solid #f0f0f0;
//  }
//
//  #container {
//    flex: 1;
//    height: 380px;
//    margin-right: 8px;
//    margin-left: 8px;
//    box-shadow: 0 0 10px 1px #e9e9e9;
//  }
//}

</style>

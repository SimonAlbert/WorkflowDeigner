<script setup lang="ts">

import { Graph } from '@antv/x6'
import {onMounted, ref} from "vue";
import {Stencil} from "@antv/x6-plugin-stencil";
const data = ref({
  nodes: [

  ],
  edges: [

  ],
})
const ports = {
  groups: {
    top: {
      position: 'top',
      attrs: {
        circle: {
          r: 4,
          magnet: true,
          stroke: '#5F95FF',
          strokeWidth: 1,
          fill: '#fff',
          style: {
            // visibility: 'hidden',
          },
        },
      },
    },
    right: {
      position: 'right',
      attrs: {
        circle: {
          r: 4,
          magnet: true,
          stroke: '#5F95FF',
          strokeWidth: 1,
          fill: '#fff',
          style: {
            // visibility: 'hidden',
          },
        },
      },
    },
    bottom: {
      position: 'bottom',
      attrs: {
        circle: {
          r: 4,
          magnet: true,
          stroke: '#5F95FF',
          strokeWidth: 1,
          fill: '#fff',
          style: {
            // visibility: 'hidden',
          },
        },
      },
    },
    left: {
      position: 'left',
      attrs: {
        circle: {
          r: 4,
          magnet: true,
          stroke: '#5F95FF',
          strokeWidth: 1,
          fill: '#fff',
          style: {
            // visibility: 'hidden',
          },
        },
      },
    },
  },
  items: [
    {
      group: 'top',
    },
    {
      group: 'right',
    },
    {
      group: 'bottom',
    },
    {
      group: 'left',
    },
  ],
}
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
  Graph.registerNode(
    'custom-rect',
    {
      inherit: 'rect',
      width: 100,
      height: 40,
      attrs: {
        body: {
          strokeWidth: 1,
          stroke: '#5F95FF',
          fill: '#EFF4FF',
        },
        text: {
          fontSize: 12,
          fill: '#262626',
        },
      },
      ports: { ...ports },
    },
    true,
  )
  Graph.registerNode(
    'custom-circle',
    {
      inherit: 'circle',
      width: 100,
      height: 40,
      attrs: {
        body: {
          strokeWidth: 1,
          stroke: '#5F95FF',
          fill: '#EFF4FF',
        },
        text: {
          fontSize: 12,
          fill: '#262626',
        },
      },
      ports: { ...ports },
    },
    true,
  )
  graph.value.on('node:click', ({ e, x, y, node, view }) => {
    console.log(e)
    console.log(x)
    console.log(y)
    console.log(node)
    console.log(view)
  })
  graph.value.on('cell:added', ({ cell, index, options }) => {
    console.log(cell)
    console.log(index)
    console.log(options)
  })


  graph.value.fromJSON(data.value)
  graph.value.centerContent()
  stencil.value = new Stencil({
    title: '流程节点',
    target: graph.value,
    collapsable: true,
    stencilGraphWidth: 300,
    stencilGraphHeight: 400,
    groups: [
      {
        title: '静态节点',
        name: 'group1',
      },
      {
        title: '动态节点',
        name: 'group2',
      },
    ],
  })
  document.getElementById('stencil')!.appendChild(stencil.value.container)
  const rect1 = graph.value.createNode({
    label: '开始',
    shape: 'custom-circle',
  })
  const rect2 = graph.value.createNode({
    label: '结束',
    shape: 'custom-circle',
  })
  const rect3 = graph.value.createNode({
    label: '用户动作',
    shape: 'custom-rect',
    // ports: {
    //   items: [
    //     {
    //       // id: 'port_1',
    //       group: 'group1',
    //     },
    //   ]
    // },
    // ports: {
    //   groups: {
    //     top: {
    //       position: 'top',
    //       group: 'top',
    //       attrs: {
    //         circle: {
    //           magnet: true,
    //           stroke: '#8f8f8f',
    //           r: 5,
    //         },
    //       },
    //     },
    //   },
    // },
  })
  const rect4 = graph.value.createNode({
    label: '系统任务',
    shape: 'custom-rect',
  })
  stencil.value.load([rect1, rect2], 'group1')
  stencil.value.load([rect3, rect4], 'group2')
})
</script>

<template>
  <div id="workflow-designer">
    <div id="stencil"></div>
    <div id="container"></div>
    <div id="props-panel">
      <h2>Node Props</h2>
      <label>处理人</label><a-input />
      <label>XXX</label><a-input />
      <label>XXX</label><a-input />
      <label>XXX</label><a-input />
      <label>XXX</label><a-input />
    </div>
  </div>
</template>

<style scoped lang="less">
#workflow-designer {
  display: flex;
  height: 100%;
  width: 100%;
  border: 1px solid #dfe3e8;
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
  #props-panel {
    padding: 10px;
    width: 400px;
    height: 600px;
    background-color: lightgray;
    position: absolute;
    right: 10px;
    top: 10px;
  }
}

</style>

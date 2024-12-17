
<script setup>
import { ref, onMounted } from 'vue'
import { VueFlow, Panel, Position, useVueFlow } from '@vue-flow/core'
import { Background } from '@vue-flow/background'

const { onConnect, addEdges, getEdges, onNodeDrag, updateNode } = useVueFlow()

onConnect((connection) => {
  addEdges(connection)
  console.log(edges)
})

onNodeDrag((dragEvent) => {
  updateNode(dragEvent.node)
})

const increment = ref(0)

const nodes = ref([
  {
    id: '1',
    position: { x: 200, y: 150 },
    class: 'not-incremented',
    data: { label: 'Node 1', increment: 1 },
  },
  {
    id: '2',
    position: { x: 50, y: 350 },
    class: 'not-incremented',
    data: { label: 'Node 2', increment: 2 },
    connectable: true
  },
  {
    id: '3',
    position: { x: 350, y: 550 },
    class: 'not-incremented',
    data: { label: 'Node 3', increment: 3 },
    connectable: true
  }
]);

const edges = ref([
  
]);

function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}
async function playIncrement() {
  increment.value = 0
  console.log(getEdges.value)  // Log the edges

  const connectedNodeIds = new Set()
  
  for (let edge of getEdges.value) {
    connectedNodeIds.add(edge.source)
    connectedNodeIds.add(edge.target)
  }

  for (let i = 0; i < nodes.value.length; i++) {
    const node = nodes.value[i]
    
    if (connectedNodeIds.has(node.id)) {
      node.class = 'incremented'
      nodes.value = [...nodes.value]
      increment.value += node.data.increment
    }
    await delay(1000)
  }
}
</script>

<template>
  <div style="height: 100vh; width: 120vh;">
    <VueFlow :nodes="nodes" :edges="edges">
      
      <Background />
      <Panel>
        <button type="button" @click="playIncrement">Play</button>
        <h1>Counter of increment: {{ increment }}</h1>
      </Panel>
    </VueFlow>
  </div>
</template>

<style>
/* these are necessary styles for vue flow */
@import '@vue-flow/core/dist/style.css';

/* this contains the default theme, these are optional styles */
@import '@vue-flow/core/dist/theme-default.css';

.not-incremented {
  background-color: blueviolet;
}
.incremented {
  background-color: greenyellow !important;
}
</style>
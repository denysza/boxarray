<script setup>
import BoxArray from './components/BoxArray.vue'
</script>

<template>
  <BoxArray :array="array" />
</template>
<script>
  export default {
    data: () => ({
      array:
       [
        {"leftID": "left1", "rightID": "right2", "priority": 1},
        {"leftID": "left2", "rightID": "right1", "priority": 2},
        {"leftID": "left2", "rightID": "right3", "priority": 3}
      ]
    })
  }
</script>
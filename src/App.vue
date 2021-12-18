<script setup>
import BoxArray from './components/BoxArray.vue'
</script>

<template>
  <BoxArray :array="array" />
</template>
<script>
  export default {
    data: () => ({
      array:{
        rightArray:{id1: "boxA", id2: "boxB"},
        leftArray:{id2: "boxD", id1: "boxC"}
      }
    })
  }
</script>
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
        rightArray:[{id1: "boxD"}, {id2: "boxE"},{id1: "boxF"},{id2: "boxG"}],
        leftArray:[{id1: "boxA"}, {id2: "boxB"},{id2: "boxC"}]        
      }
    })
  }
</script>
<script setup>
import BoxArray from './components/BoxArray.vue'
</script>


<template>
  <BoxArray :array="array" :left-array="leftArray" :right-array="rightArray" />
</template>
<script>
export default {
  data: () => ({
    array: [
      { leftID: "leftID3", rightID: "rightID", priority: 1 },
      { leftID: "leftID3", rightID: "rightID", priority: 2 },
      { leftID: "leftID3", rightID: "rightID2", priority: 3 },
    ],
    leftArray: [
      { leftID: "leftID", label: "boxD" },
      { leftID: "leftID3", label: "boxC" },
    ],
    rightArray: [
      { rightID: "rightID", label: "boxA" },
      { rightID: "rightID2", label: "boxB" },
      { rightID: "rightID3", label: "boxA" },
    ],
  }),
};
</script>

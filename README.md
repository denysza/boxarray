# Vue 3 + JointJs Boxarray Component
You can run this project after clone with this command
`$npm install`

`$npm run dev`

If you use Boxarray component in a project  please install jointjs and jquery

`$npm install jointjs`
`$npm install jquery`

Then you can use BoxArray.vue component

For example in App.vue
```
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
```


<template>
  <button @click="auth">Continue with FB</button>
</template>
<div id="fb-root"></div>
<script lang='js'>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'FBbutton',
  setup () {
    const auth = () => {
   console.log('w')
   
  }
    return { auth }
  }
})
</script>

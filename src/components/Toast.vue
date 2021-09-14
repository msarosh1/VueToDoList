<template>
  <button @click="toast">Toast it!</button>
</template>
<script lang='ts'>
import { defineComponent } from 'vue'
import { createToast } from 'mosha-vue-toastify';
import 'mosha-vue-toastify/dist/style.css'

export default defineComponent({
  name: 'HelloWorld',
  setup () {
    const toast = () => {
        createToast('HYAY, easy')
    }
    return { toast }
  }
})
</script>

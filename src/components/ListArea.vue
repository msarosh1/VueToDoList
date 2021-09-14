<template lang="js">
    <div>
    <ul>
    <li v-for="(todo, index) in todos" :key="index">
      <Editor v-model="todo.content"></Editor>
      <button class="rem-button" @click="removeItem(index)">Remove</button>
    </li>
    </ul>     
    </div>
</template>
<script>
export default {
    
}
</script>
<style lang="js">
    
</style>
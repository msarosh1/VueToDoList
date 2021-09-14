<template lang="">
    <div style="display:flex; align-content: center;">
        <button @click="GA"
        >Give Data Plz</button>
    </div>
</template>
<script>
export default {
    name: 'GA',
    methods:{
    GA() {
    
      this.$gtag.event('GA-test', {
        'event_category': 'documentation',
        'event_label': 'Just a placeholder for testing',
        'value': 1
      })
    }
    }
    
}
</script>
<style lang="css">
.button{
  cursor: pointer;
      background-color:white;
      border: 1px solid #4444;
      color: #4444;
      font-weight: bold;
      outline: none;
      margin: 1em 0em 0em 0em;
      padding: 0;
      
}
    
</style>
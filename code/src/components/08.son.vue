<template>
  <div class="son">
    <h1>子组件</h1>
    <grandson></grandson>
  </div>
</template>
<script>
    import grandson from './08.grandson.vue'
    export default {
        setup(){
        },
        components: {
            grandson
        }
    }
</script>




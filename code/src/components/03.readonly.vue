<template>
  <div class="hello">
    <p>{{readNum}}</p>
    <button @click="onChange">修改readNum</button>
  </div>
</template>
<script>
    import {ref,readonly} from 'vue'
    export default {
        setup(){
            const num = ref(1)
           let readNum = readonly(num)
            let onChange = ()=>{
                readNum.value = 10;
            }
            return{
                readNum,
                onChange
            }
        }
    }
</script>




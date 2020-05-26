<template>
  <div class="hello">
    <p>{{obj.name}}</p>
    <button @click="onChange">修改name</button>
  </div>
</template>
<script>
    import {reactive} from 'vue'
    export default {
        setup(){
            let obj = reactive({
                name:'tom',
                age:20
            });
            let onChange = ()=>{
                obj.name = "Json";
            }
            return{
                obj,
                onChange
            }
        }
    }
</script>




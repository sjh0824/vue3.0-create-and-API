<template>
  <div>
    <p>obj：{{obj.name}}</p>
    <p>obj：{{obj.num}}</p>
    <p>objFlag：{{objFlag}}</p>
    <button @click="onChange">修改num</button>
  </div>
</template>
<script>
    import {reactive,toRefs} from 'vue'
    export default {
        setup(){

            let obj = reactive({
                name:'tom',
                num:1
            })
            let objFlag = toRefs(obj);
            const onChange = ()=>{
                objFlag.name.value = 'Json';
                objFlag.num.value = 10;
                console.log(obj.num) //10
                console.log(obj.name) //Json
            }
            return{
                objFlag,
                obj,
                onChange
            }
        },
    }
</script>




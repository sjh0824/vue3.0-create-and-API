<template>
  <div>
    <p>obj：{{obj.num}}</p>
    <p>objFlag：{{objFlag}}</p>
    <button @click="onChange">修改num</button>
  </div>
</template>
<script>
    import {reactive,toRef} from 'vue'
    export default {
        setup(){

            let obj = reactive({
                age:5,
                num:1
            })
            let objFlag = toRef(obj,'num');
            const onChange = ()=>{
                objFlag.value = 10;
                console.log(obj.num) //10
                console.log(objFlag.value) //10
            }
            return{
                objFlag,
                obj,
                onChange
            }
        },
    }
</script>




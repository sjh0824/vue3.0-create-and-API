<template>
  <div>
    <p>numFlag：{{numFlag}}</p>
    <p>singleFlag：{{singleFlag}}</p>
    <p>objFlag：{{objFlag}}</p>
  </div>
</template>
<script>
    import {ref,reactive,isReadonly,readonly} from 'vue'
    export default {
        setup(){
            let num = ref(5);
            let single = readonly({
                name:'tom'
            });
            let obj = reactive({
                age:5
            })
            let numFlag = isReadonly(num);  //false
            let singleFlag = isReadonly(single); //true
            let objFlag = isReadonly(obj); //false
            return{
                numFlag,
                singleFlag,
                objFlag
            }
        },
    }
</script>




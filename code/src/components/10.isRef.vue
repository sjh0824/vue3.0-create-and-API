<template>
  <div>
    <p>numFlag：{{numFlag}}</p>
    <p>singleFlag：{{singleFlag}}</p>
    <p>objFlag：{{objFlag}}</p>
  </div>
</template>
<script>
    import {ref,reactive,isRef} from 'vue'
    export default {
        setup(){
            let num = ref(5);
            let single = 5;
            let obj = reactive({
                age:5
            })
            let numFlag = isRef(num);  //true
            let singleFlag = isRef(single); //false
            let objFlag = isRef(obj); //false
            return{
                numFlag,
                singleFlag,
                objFlag
            }
        },
    }
</script>




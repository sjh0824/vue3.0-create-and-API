<template>
  <div>
    <p>numFlag：{{numFlag}}</p>
    <p>singleFlag：{{singleFlag}}</p>
    <p>objFlag：{{objFlag}}</p>
  </div>
</template>
<script>
    import {ref,reactive,isReactive} from 'vue'
    export default {
        setup(){
            let num = ref(5);
            let single = {num:5};
            let obj = reactive({
                age:5
            })
            let numFlag = isReactive(num);  //false
            let singleFlag = isReactive(single); //false
            let objFlag = isReactive(obj); //true
            return{
                numFlag,
                singleFlag,
                objFlag
            }
        },
    }
</script>




<template>
  <div>
    <p>numFlag：{{numFlag}}</p>
    <p>singleFlag：{{singleFlag}}</p>
    <p>objFlag：{{objFlag}}</p>
  </div>
</template>
<script>
    import {ref,reactive,isProxy,readonly} from 'vue'
    export default {
        setup(){
            let num = ref(5);
            let single = readonly({
                name:'tom'
            });
            let obj = reactive({
                age:5
            })
            let numFlag = isProxy(num);  //false
            let singleFlag = isProxy(single); //true
            let objFlag = isProxy(obj); //true
            return{
                numFlag,
                singleFlag,
                objFlag
            }
        },
    }
</script>




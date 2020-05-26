<template>
  <div>
    <p>numFlag：{{numFlag}}</p>
    <p>singleFlag：{{singleFlag}}</p>
    <p>objFlag：{{objFlag}}</p>
  </div>
</template>
<script>
    import {ref,reactive,unref,readonly} from 'vue'
    export default {
        setup(){
            let num = ref(5);
            let single = readonly({
                name:'tom'
            });
            let obj = reactive({
                age:5
            })
            let numFlag = unref(num);  //5
            let singleFlag = unref(single); //{name:'tom'}
            let objFlag = unref(obj); //{age:5}
            return{
                numFlag,
                singleFlag,
                objFlag
            }
        },
    }
</script>




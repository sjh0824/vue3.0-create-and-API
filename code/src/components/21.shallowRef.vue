<template>
  <div>
    <p>name：{{obj}}</p>
    <!--<p>age：{{obj.infos.age}}</p>-->
  </div>
</template>
<script>
    import {ref,shallowRef,isReactive} from 'vue'
    export default {
        setup(){
           let obj = shallowRef({});
           let infos = ref({});
            obj.value = {};
            infos.value = {}
            console.log(isReactive(obj)) ; //false
            console.log(isReactive(infos)) ; //false
           return {
               obj
           }
        },
    }
</script>




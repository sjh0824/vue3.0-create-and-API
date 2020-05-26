<template>
  <div>
    <p>name：{{obj}}</p>
    <!--<p>age：{{obj.infos.age}}</p>-->
  </div>
</template>
<script>
    import {reactive,readonly,toRaw} from 'vue'
    export default {
        setup(){
           let obj = {};
           let resactives = reactive(obj);
           let readonlys = readonly(obj);
           console.log( toRaw(resactives) === obj)  //true
           console.log( toRaw(readonlys) === obj)  //true
        }
    }
</script>




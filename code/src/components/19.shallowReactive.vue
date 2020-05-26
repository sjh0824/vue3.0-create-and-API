<template>
  <div>
    <p>name：{{obj.name}}</p>
    <p>age：{{obj.infos.age}}</p>
  </div>
</template>
<script>
    import {shallowReactive,isReactive} from 'vue'
    export default {
        setup(){
           let obj = shallowReactive({
               name:'tom',
               infos:{
                   age:10
               }
           });
            // obj中的属性是响应式的，可被传递
            console.log(isReactive(obj)) ; //true
            // 而obj.infos并不是响应式的
            console.log(isReactive(obj.infos));  //false
           setTimeout(()=>{
               obj.name = "Json";
               obj.infos.age++;  //数值变为11了？
               console.log(obj)
           },2000);
           return {
               obj
           }
        },
    }
</script>




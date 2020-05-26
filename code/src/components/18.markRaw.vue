<template>
  <div>
  </div>
</template>
<script>
    import {markRaw,reactive,isReactive} from 'vue'
    export default {
        setup(){
           let obj = markRaw({
               name:'tom',
               infos:{
                   age:10
               }
           })
            console.log(isReactive(reactive(obj)))  //false
            // 尽管 `obj` 己经被标记为 markRaw 了, 但 markRaw.infos 并没有
            console.log(isReactive(reactive(obj.infos)))  //true
            // 如果被 markRaw 标记了，即使在响应式对象中作属性，也依然不是响应式的
            const objTemp = reactive({ obj })
            console.log(isReactive(objTemp.foo)) // false
            console.log(isReactive(objTemp)) // true
            return{

            }
        },
    }
</script>




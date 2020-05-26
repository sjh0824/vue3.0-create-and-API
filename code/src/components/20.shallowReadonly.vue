<template>
  <div>
    <p>name：{{obj.name}}</p>
    <p>age：{{obj.infos.age}}</p>
  </div>
</template>
<script>
    import {shallowReadonly,isReadonly} from 'vue'
    export default {
        setup(){
           let obj = shallowReadonly({
               name:'tom',
               infos:{
                   age:10
               }
           });
            // obj中的属性是只读的，可被传递
            console.log(isReadonly(obj)) ; //true
            // 而obj.infos并不是只读的
            console.log(isReadonly(obj.infos));  //false
           setTimeout(()=>{
               obj.name = "Json";  //Set operation on key "name" failed: target is readonly.
               obj.infos.age++;  //数值变为11了,但是页面显示原来的数据？
               console.log(obj)
           },2000);
           return {
               obj
           }
        },
    }
</script>




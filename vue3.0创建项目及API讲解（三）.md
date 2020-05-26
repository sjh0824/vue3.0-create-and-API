# 高级响应式系统 API

#### 1、customRef
用于自定义一个 ref，可以显式地控制依赖追踪和触发响应，接受一个工厂函数，两个参数分别是用于追踪的 track 与用于触发响应的 trigger，并返回一个一个带有 get 和 set 属性的对象。
```javascript
<template>
  <div>
    <p>refNum：{{refNum}}</p>
  </div>
</template>
<script>
import {customRef} from 'vue'
export default {
    setup(){
        const cusRefs = (value)=>{
            return customRef((track,trigger)=>{
                return {
                    get(){
                        track()
                        return value
                    },
                    set(newValue){
                        value = newValue;
                        trigger();
                    }
                }
            })
        }
        const refNum = cusRefs(4)
        setTimeout(()=>{
            refNum.value = 10;
        },3000)
        return{
            refNum
        }
    },
}
</script>
```


#### 2、markRaw
显式标记一个对象为“永远不会转为响应式代理”，函数返回这个对象本身。
```javascript
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
​
        }
    },
}
```


#### 3、shallowReactive
只为某个对象的私有（第一层）属性创建浅层的响应式代理，不会对“属性的属性”做深层次、递归地响应式代理，而只是保留原样。
```javascript
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
               obj.infos.age++;  //数值变为11了，页面上也显示11？
               console.log(obj)
           },2000);
           return {
               obj
           }
        },
    }
</script>
```


#### 4、shallowReadonly
只为某个对象的自有（第一层）属性创建浅层的只读响应式代理，同样也不会做深层次、递归地代理，深层次的属性并不是只读的。
```javascript
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
```


#### 5、shallowRef
创建一个 ref对象，将会追踪它的 .value 更改操作，但是并不会对变更后的 .value 做响应式代理转换（即变更不会调用 reactive）
```javascript
import{ref,shallowRef,isReactive} from 'vue'
export default {
    setup(){
       let obj = shallowRef({});
       let infos = ref({});
        obj.value = {};
        infos.value = {}
        console.log(isReactive(obj)) ; //false
        console.log(isReactive(infos)) ; //false
    },
}
```


#### 6、toRaw
返回由 reactive 或 readonly 方法转换成响应式代理的普通对象。这是一个还原方法，可用于临时读取，访问不会被代理/跟踪，写入时也不会触发更改。不建议一直持有原始对象的引用。请谨慎使用。
```javascript
import{reactive,readonly,toRaw} from 'vue'
export default {
    setup(){
       let obj = {};
       let resactives = reactive(obj);
       let readonlys = readonly(obj);
       //还原reactive/readonly对象
       console.log( toRaw(resactives) === obj)  //true
       console.log( toRaw(readonlys) === obj)  //true
    }
}
```


欢迎关注公众号（web学习吧），一起学习进步:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200526170903347.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjMxMDc4,size_16,color_FFFFFF,t_70)

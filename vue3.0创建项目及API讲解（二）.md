
## 一、依赖注入
依赖注入就是祖先组件向后代组件传递数据，使用provide()和inject()函数来实现
这两个函数只能在setup()函数中使用
在祖先组件中使用provide()函数向下传递数据
在后代组件中使用inject()函数获取上层传递传递过来的数据

```javascript
//父组件
<template>
  <div class="hello">
    <h1>父组件</h1>
    <son></son>
    <button @click="colorRef='red'">红</button>
    <button @click="colorRef='yellow'">黄</button>
    <button @click="colorRef='blue'">蓝</button>
  </div>
</template>
<script>
    import { provide,ref} from 'vue'
    import son from './son.vue'
    export default {
        setup(){
            // 向后代传数据  不会响应式更新
            provide('color','yellow');
            // 传递响应式的数据
            let colorRef = ref('red')
            provide('colorRef',colorRef);
            return{
                colorRef
            }
        },
        components: {
            son
        }
    }
</script>

//子组件
<template>
  <div class="son">
    <h1>子组件</h1>
    <grandson></grandson>
  </div>
</template>
<script>
    import grandson from './grandson.vue'
    export default {
        setup(){
        },
        components: {
            grandson
        }
    }
</script>


//孙子组件
<template>
  <div class="grandson">
    <h1 :style="{color:color}">孙组件1</h1>
    <h1 :style="{color:colorRef}">孙组件1</h1>
  </div>
</template>
<script>

    import { inject} from 'vue'
    export default {
        setup(){
            const color = inject('color');
            const colorRef = inject('colorRef');
            return {
                color,
                colorRef
            }
        }
    }
</script>
```


## 二、模板Refs

通过ref()函数还可以引用页面上的元素或组件
步骤：
1、在setup()中创建一个ref()对象并返回
2、在页面上为元素添加ref属性，并设置属性值与创建的ref对象的名称相同
 3、当前页面渲染完成后（onMounted），可以通过该ref对象获取到页面中对应的dom元素

```javascript
//refs.vue
<template>
  <div>
    <h1 ref="hRef">这是一段文字，需要获取</h1>
    <hr>
    <compRefs ref="comRef" :onChanges="onChanges"></compRefs>
  </div>
</template>
<script>
    import {ref,onMounted} from 'vue'
    import compRefs from './compRef.vue'
    export default {
        setup(){
            // 创建一个值为null的ref对象并返回
            const hRef = ref(null);
            // 获取组件
            const comRef = ref(null);
            // 在页面渲染完成后
            onMounted(()=>{
                console.log(hRef.value) //DOM对象
                console.log(comRef.value) //DOM对象
                hRef.value.style.color = 'red'
                comRef.value.onChange();  //父组件调用子组件方法
            })
            let onChanges = ()=>{
                hRef.value.innerHTML = ' hello'
            }
            return{
                hRef,
                comRef,
                onChanges
            }
        },
        components:{
            compRefs
        }
    }
</script>


//compRefs.vue
<template>
  <div>
   <p>{{msg}}</p>
    <button @click="childMethod">点击</button>
  </div>
</template>
<script>
    import {ref,onMounted} from 'vue'
    export default {
        props:{
            onChanges: Function
        },
        setup(props){
            const msg = ref('welcome');

            let onChange = ()=>{
                msg.value += ' hello'
            }
            let childMethod = ()=>{
                console.log(props)
                props.onChanges();  //子组件调用父组件方法
            }
            onMounted(()=>{
            })
            return{
                msg,
                onChange,
                childMethod
            }
        }
    }
</script>
```


## 三、响应式系统工具集

#### 1、isRef
 检查一个值是否为一个 ref 对象。

```javascript
import{ref,reactive,isRef} from 'vue'
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
```


#### 2、isReactive
 检查一个对象是否是由 reactive 创建的响应式代理。

```javascript
import{ref,reactive,isReactive} from 'vue'
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
```


#### 3、isReadonly
检查一个对象是否是由 readonly 创建的只读代理。
```javascript
import {ref,reactive,isReadonly,readonly} from 'vue'
export default {
    setup(){
        let num = ref(5);
        let single = readonly({
            name:'tom'
        });
        let obj = reactive({
            age:5
        })
        let numFlag = isReadonly(num);  //false
        let singleFlag = isReadonly(single); //true
        let objFlag = isReadonly(obj); //false
        return{
            numFlag,
            singleFlag,
            objFlag
        }
    },
}
```


#### 4、isProxy
检查一个对象是否是由 reactive 还是 readonly 方法创建的代理。

```javascript
import {ref,reactive,isProxy,readonly} from'vue'
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
```


#### 5、unref

 如果参数是一个 ref 则返回它的 value，否则返回参数本身。它是 val = isRef(val) ? val.value : val 的语法糖。注意：unref都是小写的

```javascript
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
```


#### 6、toRef
用来为一个 reactive 对象的属性创建一个 ref。这个 ref 可以被传递并且能够保持响应性，可以被传递。

```javascript
<template>
  <div>
    <p>obj：{{obj.num}}</p>
    <p>objFlag：{{objFlag}}</p>
    <button @click="onChange">修改num</button>
  </div>
</template>
<script>
    import {reactive,toRef} from 'vue'
    export default {
        setup(){
            let obj = reactive({
                age:5,
                num:1
            })
            //只能是reactive对象中的一个属性
            let objFlag = toRef(obj,'num');
            const onChange = ()=>{
                objFlag.value = 10;
                console.log(obj.num) //10
                console.log(objFlag.value) //10
            }
            return{
                objFlag,
                obj,
                onChange
            }
        },
    }
</script>
```


#### 7、toRefs
 用来将reactive对象转换为一个普通对象，但该普通对象的每个 属性都是一个 ref ，并且这个ref具有响应式，可以被传递。
```javascript
<template>
  <div>
    <p>obj：{{obj.name}}</p>
    <p>obj：{{obj.num}}</p>
    <p>objFlag：{{objFlag}}</p>
    <button @click="onChange">修改num</button>
  </div>
</template>
<script>
    import {reactive,toRefs} from 'vue'
    export default {
        setup(){

            let obj = reactive({
                name:'tom',
                num:1
            })
            let objFlag = toRefs(obj);
            const onChange = ()=>{
                objFlag.name.value = 'Json';
                objFlag.num.value = 10;
                console.log(obj.num) //10
                console.log(obj.name) //Json
            }
            return{
                objFlag,
                obj,
                onChange
            }
        },
    }
</script>
```

欢迎关注公众号（web学习吧），一起学习进步:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200526160443356.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjMxMDc4,size_16,color_FFFFFF,t_70)
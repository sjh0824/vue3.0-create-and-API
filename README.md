# 一、项目创建

## 1、检查vue-cli脚手架版本（vue -V），低版本的要更新（npm install @vue/cli -g）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525193045876.png)

## 2、创建项目（vue create vue3test ）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525193103995.png)
#### 选择default（直接回车enter）,初始化项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525193110309.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525193139345.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjMxMDc4,size_16,color_FFFFFF,t_70)
## 3、进入项目文件夹，更新vue版本（vue add vue-next）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525193148108.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjMxMDc4,size_16,color_FFFFFF,t_70)
## 4、运行项目（npm run serve）
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020052519315567.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525193202197.png)
## 5、浏览器上访问（http://localhost:8081），初始化页面展示



# 二、Vue3API

        vue3主要是低侵入式的、函数式的 API，需要用到的函数都必须提前引入。例如：引入ref函数：import { ref} from 'vue'

## 1、setup函数
返回一个对象，则对象的属性将会被合并到组件模板的渲染上下文。
其他所有的引入函数都必须在其内部执行。
创建组件实例，然后初始化 props ，紧接着就调用setup 函数。从生命周期钩子的视角来看，它会在 beforeCreate 钩子之前被调用

```javascript
<script>
export default {
    setup(){
        console.log("steup")
    },
    beforeCreate(){
        console.log("beforeCreate")
    },
    created(){
        console.log("created")
    }

}
</script
```

浏览器打印顺序：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525192952279.png)

## 2、响应式系统 API

#### （1）、ref

接受一个参数值并返回一个响应式且可改变的 ref 对象。ref 对象拥有一个指向内部值的单一属性 .value。主要用于数字、字符串的创建。
	动态修改ref对象的值时，需要利用.value去修改。但是setup 返回的 ref 在模板中会自动解开，不需要写 .value。
	点击按钮，展示的num的值就会变为9999

```javascript
<template>
  <div class="hello">
      <p>{{num}}</p>
      <p>{{str}}</p>
    <button @click="onChange">修改num</button>
  </div>
</template>
<script>
  import {ref} from 'vue'
export default {
    setup(){
        let num = ref(333)
        let str = ref('fffff');
        let onChange = ()=>{
            num.value = 9999;
        }
        return{
            num,
            str,
            onChange
        }
    }
}
</script>
```


#### （2）、reactive
接收一个普通对象然后返回该普通对象的响应式代理。
```javascript
<template>
  <div class="hello">
    <p>{{obj.name}}</p>
    <button @click="onChange">修改name</button>
  </div>
</template>
<script>
    import {reactive} from 'vue'
    export default {
        setup(){
            let obj = reactive({
                name:'tom',
                age:20
            });
            let onChange = ()=>{
                obj.name = "Json";
            }
            return{
                obj,
                onChange
            }
        }
    }
</script>
```


#### （3）、computed
传入一个 getter 函数，返回一个默认不可手动修改的 ref 对象。
		当点击按钮时，proNum不变，浏览器会提示‘Write operation failed: computed value is readonly’
```javascript
<template>
  <div class="hello">
    <p>{{proNum}}</p>
    <button @click="onChange">修改proNum</button>
  </div>
</template>
<script>
    import {ref,computed} from 'vue'
    export default {
        setup(){
            const num = ref(5)
            const proNum = computed(() => num.value + 1)
            let onChange = ()=>{
                proNum.value += 10;
            }
            return{
                proNum,
                onChange
            }
        }
    }
</script>
```
如果要computed 可以修改值，则需要传入一个拥有 get 和 set 函数的对象，创建一个可手动修改的计算状态。
```javascript
<template>
  <div class="hello">
    <p>{{num}}</p>
    <button @click="onChange">修改proNum</button>
  </div>
</template>
<script>
    import {ref,computed} from 'vue'
    export default {
        setup(){
            const num = ref(1)
            const proNum = computed({
                set:(val)=>{
                    num.value = val+10;
                },
                get:()=>{
                    num.value
                }
            })
            let onChange = ()=>{
                proNum.value = 10;
                console.log(num)
            }
            return{
                num,
                onChange
            }
        }
    }
</script>
```


#### （4）、readonly
传入一个对象（响应式或普通）或 ref，返回一个原始对象的只读代理。一个只读的代理是“深层的”，对象内部任何嵌套的属性也都是只读的。
        点击按钮，浏览器中提示‘Set operation on key "value" failed: target is readonly’
```javascript
<template>
  <div class="hello">
    <p>{{readNum}}</p>
    <button @click="onChange">修改readNum</button>
  </div>
</template>
<script>
    import {ref,readonly} from 'vue'
    export default {
        setup(){
            const num = ref(1)
           let readNum = readonly(num)
            let onChange = ()=>{
                readNum.value = 10;
            }
            return{
                readNum,
                onChange
            }
        }
    }
</script>
```


#### （5）、watch
需要侦听特定的数据源，并在回调函数中执行副作用。默认情况是懒执行的，也就是说仅在侦听的数据源变更时才执行回调。

```javascript
<template>
  <div class="hello">
    <p>{{num}}</p>
    <p>{{state.name}}</p>
    <button @click="onChange">修改num-name</button>
  </div>
</template>
<script>
    import {ref,reactive,watch} from 'vue'
    export default {
        setup(){
            // 侦听一个 reactive  一个数据源
            const state = reactive({ name: 'tom' });
            watch(
                () => state.name,
                (name, prevName) => {
                    console.log(prevName,name)
                }
            );
            // 直接侦听一个 ref   一个数据源
            const num = ref(1);
            watch(num, (num, prevNum) => {
               console.log(prevNum,num)
            });

            // 侦听多个数据源
            watch([num,state], ([num,name], [prevNum,prevName]) => {
                console.log(prevNum,num+"-----"+prevName.name,name.name)
            });
            let onChange = ()=>{
                state.name = 'Json';
                num.value =  10;
            };
            return{
                state,
                num,
                onChange
            }
        }
    }
</script>
```

#### （6）、watchEffect
立即执行传入的一个函数，并响应式追踪其依赖，并在其依赖变更时重新运行该函数。
```javascript
<template>
  <div class="hello">
    <p>{{num}}</p>
  </div>
</template>
<script>
    import {ref,watchEffect} from 'vue'
    export default {
        setup(){
            const num = ref(1);
            const stop = watchEffect(()=>{
                console.log(num.value)
            })
            setTimeout(()=>{
                num.value = 10
            },2000)

            // 停止侦听  调用stop 浏览器上不会打印 10
            stop();
            return{
                num
            }
        }
    }
</script>
```

## 三、生命周期钩子函数
```javascript
与 2.x 版本生命周期相对应的组合式 API
beforeCreate -> 使用 setup()
created -> 使用 setup()
beforeMount -> onBeforeMount
mounted -> onMounted
beforeUpdate -> onBeforeUpdate
updated -> onUpdated
beforeDestroy -> onBeforeUnmount
destroyed -> onUnmounted
```
这些生命周期钩子注册函数只能在 setup() 期间同步使用， 因为它们依赖于内部的全局状态来定位当前组件实例（正在调用 setup() 的组件实例）, 不在当前组件下调用这些函数会抛出一个错误。
组件实例上下文也是在生命周期钩子同步执行期间设置的，因此，在卸载组件时，在生命周期钩子内部同步创建的侦听器和计算状态也将自动删除。
```javascript
<template>
  <div class="hello">
    <p>{{num}}</p>
    <button @click="onChange">修改num</button>
  </div>
</template>
<script>
    import {ref,onBeforeMount,onMounted,onBeforeUpdate,onUpdated,onBeforeUnmount,onUnmounted} from 'vue'
    export default {
        setup(){
            onBeforeMount(() => {
                console.log('onBeforeMount!')
            });
            onMounted(() => {
                console.log('mounted!')
            });
            onBeforeUpdate(() => {
                console.log('onBeforeUpdate!')
            });
            onUpdated(() => {
                console.log('updated!')
            });
            onBeforeUnmount(() => {
                console.log('onBeforeUnmount!')
            });
            onUnmounted(() => {
                console.log('unmounted!')
            });
            const num = ref(1);
            let onChange = ()=>{
                num.value = 10;
            };
            return{
                num,
                onChange
            }
        }
    }
</script>
```


欢迎关注公众号（web学习吧），一起学习进步:
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020052519352564.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjMxMDc4,size_16,color_FFFFFF,t_70)
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




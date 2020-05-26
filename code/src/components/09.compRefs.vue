<template>
  <div>
    <h1 ref="hRef">这是一段文字，需要获取</h1>
    <hr>
    <compRefs ref="comRef" :onChanges="onChanges"></compRefs>
  </div>
</template>
<script>
    import {ref,onMounted} from 'vue'
    import compRefs from './09.compRef.vue'
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




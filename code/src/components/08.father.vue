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
    import son from './08.son.vue'
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




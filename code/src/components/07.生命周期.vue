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




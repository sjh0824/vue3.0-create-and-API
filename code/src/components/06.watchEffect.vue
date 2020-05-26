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




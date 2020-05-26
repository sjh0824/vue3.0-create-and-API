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




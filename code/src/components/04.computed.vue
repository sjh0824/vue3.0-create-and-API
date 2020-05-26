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




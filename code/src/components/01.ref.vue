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
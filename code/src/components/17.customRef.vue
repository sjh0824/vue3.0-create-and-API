<template>
  <div>
    <p>refNum：{{refNum}}</p>
  </div>
</template>
<script>
    import {customRef} from 'vue'
    export default {
        setup(){
            const cusRefs = (value)=>{
                return customRef((track,trigger)=>{
                    return {
                        get(){
                            track()
                            return value
                        },
                        set(newValue){
                            value = newValue;
                            trigger();
                        }
                    }
                })
            }
            const refNum = cusRefs(4)
            setTimeout(()=>{
                refNum.value = 10;
            },3000)
            return{
                refNum
            }
        },
    }
</script>




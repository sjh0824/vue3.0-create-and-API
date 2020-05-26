<template>
  <div class="grandson">
    <h1 :style="{color:color}">孙组件1</h1>
    <h1 :style="{color:colorRef}">孙组件1</h1>
  </div>
</template>
<script>

    import { inject} from 'vue'
    export default {
        setup(){
            const color = inject('color');
            const colorRef = inject('colorRef');
            return {
                color,
                colorRef
            }
        }
    }
</script>




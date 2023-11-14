<template>
  <div class="editor-container">
    <div id="ctrl">PROBAR ARCHIVO AQUI</div>
    <div>{{ testPath }}</div>
  </div>
</template>

<script>
import MenuBar from '@/components/TitleBar.vue';




export default {
  name:'EditorView',
  emits:['clicked'],
  components:{
    MenuBar,
  },
  data() {
    return {
      testPath:null,
    }
  },
  computed:{
  },
  created(){},
  mounted(){
    
  },
  methods:{
    test(){
      console.log('Test Function')
    },
  },
  watch:{
  }
}
</script>

<style lang='sass'>
</style>
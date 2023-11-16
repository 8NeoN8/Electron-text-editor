<template>
  <div class="editor-container">

    <FileExplorer/>

  </div>
</template>

<script>
import FileExplorer from '@/components/FileExplorer.vue';




export default {
  name:'EditorView',
  emits:[],
  components:{
    FileExplorer,
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

<style lang='scss'>
</style>
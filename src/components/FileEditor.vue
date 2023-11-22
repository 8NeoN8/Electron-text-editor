<template>
  <div class="opened-file-editor" v-for="(file, index) in openedFiles" :key="index">
    <div v-if="file == focusedFile" style="color:red; margin:1rem;" contenteditable="true">
      <textarea class="file-editor-textarea">
        {{ Object.entries(filesContents) }}
      </textarea>
    </div>
  </div>
</template>

<script>
import fs from 'fs'

export default {
  name:'FileEditor',
  components:{},
  data() {
    return {
      filesContents:{},
    }
  },
  computed:{
    workAroundIHope(){

    }
  },
  props:{
    openedFiles:{
      type: Object,
      default: {}
    },
    focusedFile:{
      type:Object,
      default: {}
    }
  },
  created(){
  },
  mounted(){
  },
  methods:{

    //! AYUDA CAUSA ME ESTOY VOLVIENDO LOCO
    readAdnSetFiles(filesObject){
      Object.entries(filesObject).forEach(element => {
        fs.readFile(element[1].filePath,'utf8',(err,data)=>{
          this.filesContents[element[0]] = data
          console.log(this.filesContents);
        })
      });
    }
  },
  watch:{
  },
  updated() {
    //! ESTO CASI FUNCIONA PERO TRATA CON OTRAS FORMAS DESDE CERO TAMBIEN
    this.readAdnSetFiles(this.openedFiles)
    console.log(this.openedFiles);
  },
}
</script>

<style lang='scss'>
@import '@/styles/fileEditor.scss'
</style>
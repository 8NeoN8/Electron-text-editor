<template>
  <div class="editor-container">
    
    <FileExplorer 
      v-if="showExplorer"
      @file-clicked="handleFileClick($event)"
    />

    <!-- <div class="editor-test">
      <button type="button" class="toggle-explorer" @click="showExplorer = !showExplorer">
        Toggle Explorer
      </button>
    </div> -->

    <div class="file-editor-and-terminal-container">
    
      <FileEditorTab :openedFiles="openedFiles" :focusedFile="focusedFile" @clickedTab="handleTabClick($event)" @close-tab="handleCloseTab($event)"/>
      <FileEditor :openedFiles="openedFiles" :focusedFile="focusedFile"/>

    </div>

    

  </div>
</template>

<script>
import FileExplorer from '@/components/FileExplorer.vue';
import FileEditorTab from '@/components/FileEditorTab.vue';
import FileEditor from '@/components/FileEditor.vue';
import fs from 'fs'


export default {
  name:'EditorView',
  emits:[],
  components:{
    FileExplorer,
    FileEditor,
    FileEditorTab
  },
  data() {
    return {
      testPath:null,
      showExplorer: true,
      focusedFile: null,
      openedFiles:{},
    }
  },
  computed:{
  },
  created(){},
  mounted(){
    
  },
  methods:{
    handleFileClick(file){
      const copyFile = Object.assign(file)
      
      this.openedFiles[copyFile.filename] = copyFile
      this.focusedFile = copyFile
      //console.log('File focused: ', copyFile.filename);
      
      /* fs.readFile(file.filePath,(err,data) => {
        if(err) return console.log('Error! ', err);  
        console.log(data.toString());
      }) */
    
    },
    
    handleTabClick(tabContent){
      this.focusedFile = tabContent.file
    },
    handleCloseTab(tabContent){
      if(this.focusedFile == tabContent.file) this.focusedFile == {}
      delete this.openedFiles[tabContent.file.filename]
    }
    
  },
  watch:{
  }
}
</script>

<style lang='scss'>

@import '@/styles/editorView.scss';
</style>
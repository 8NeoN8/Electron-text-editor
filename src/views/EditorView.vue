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

      <!-- Refactorizar a que sea un loop y dependiendo de las open files le paso a cada uno una file y se hace el editor y listo, y que la visivilidad sea segun el focusedFile -->
      <template v-for="(file, index) in openedFiles" :key="index">

        <FileEditor :openedFile="file" :fileIndex="index" :focusedFile="focusedFile"/>
          
      </template>



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
      openedFilesQueue: [],
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

      //TODO this implies that it does not support two files with the same name to be opened even if they have different paths, asses this later
      if(!this.openedFiles[copyFile.filename]) this.openedFiles[copyFile.filename] = copyFile
      this.focusedFile = copyFile
      //console.log('File focused: ', copyFile.filename);

      /* if(this.openedFilesQueue.length == 0){
        this.openedFilesQueue.push(file.filePath)
        return
      }

      if(this.openedFilesQueue.length == 1 && this.openedFilesQueue[0] != file.filePath){
        this.openedFilesQueue.push(file.filePath)
        return
      }

      if(this.openedFilesQueue.length > 1){
        
      } */
      
      
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
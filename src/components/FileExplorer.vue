<template>
  <div id="file-explorer" class="file-explorer" resizable='true'>

    <div class="no-drag">
      <div class="explorer-buttons">
        <button>file</button>
        <button>directory</button>
        <button>refresh</button>
        <button id="home-path-input">open</button>
      </div>

      <div class="explorer-current-directory">
        {{ pathVisible }}
      </div>

      <div class="file-tree-container">
        <FileTree 
          v-for="(file, index) in directoryFiles" 
          :key="index" 
          :fileNode="file"
          @file-clicked="handleFileClick($event)"
        />
      </div>
    </div>

    <div id="drag" class="drag">
    </div>

  </div>
</template>

<script>
import FileTree from "@/components/FileTree.vue";

const remote = require("@electron/remote");
const fs = require('fs')
const path = require('path')

export default {
  name:'FileExplorer',
  emits:['file-clicked'],
  components:{
    FileTree
  },
  props:{
    /* directoryPath:{
      type: String,
      default:'C:\\'
    } */
  },
  data() {
    return {
      directoryFiles: [],
      directoryPath: 'C:\\'
    }
  },
  computed:{
    pathVisible(){
      let visible = this.directoryPath;
      if(this.directoryPath != 'C:\\') return visible.split('\\').pop()
      else return 'C\\'
    }
  },
  created(){
  },
  mounted(){
    this.applyResize()
    this.setOpenDirectory()

  },
  methods:{
    applyResize(){
      let p = document.querySelector('#file-explorer'); // element to make resizable
      /* p.style.minWidth = '10%'
      p.style.maxWidth = '30%' */

      p.addEventListener('mouseenter', function init() {
          p.removeEventListener('click', init, false);
          p.className = p.className + ' resizable';
          let resizer = document.createElement('div');
          resizer.className = 'resizer';
          p.appendChild(resizer);
          resizer.addEventListener('mousedown', initDrag, false);
      }, false);

      let startX, startY, startWidth, startHeight;

      function initDrag(e) {
        startX = e.clientX;
        //startY = e.clientY;
        startWidth = parseInt(document.defaultView.getComputedStyle(p).width, 10);
        //startHeight = parseInt(document.defaultView.getComputedStyle(p).height, 10);
        document.documentElement.addEventListener('mousemove', doDrag, false);
        document.documentElement.addEventListener('mouseup', stopDrag, false);
      }

      function doDrag(e) {
        p.style.width = (startWidth + e.clientX - startX) + 'px';
        //p.style.height = (startHeight + e.clientY - startY) + 'px';
      }

      function stopDrag(e) {
          document.documentElement.removeEventListener('mousemove', doDrag, false);
          document.documentElement.removeEventListener('mouseup', stopDrag, false);
      }
    },
    setOpenDirectory(){
      let input = document.getElementById('home-path-input')
      input.addEventListener('click', async (event)=>{
        let homePath = (await remote.dialog.showOpenDialog({properties: ['openDirectory', 'multiSelections']})).filePaths[0]
        if(homePath) this.directoryPath = homePath
      })
    },
    getDirectoryFiles(dir, fileList = []){
      fs.readdirSync(dir).forEach(file => {
        if(file == 'node_modules') return
        const dirFile = path.join(dir, file);
        let fileObject = {
          filePath:dirFile,
          filename:file,
          isDirectory: fs.statSync(dirFile).isDirectory(),
          subDirectories:[],
          showSubDirectories: false,
        }
        try {
          fileObject.subDirectories = this.getDirectoryFiles(fileObject.filePath, fileObject.subDirectories);
        } catch (err) {
          if (err.code === 'ENOTDIR' || err.code === 'EBUSY' || err.code === 'ENOENT') fileList = [...fileList, dirFile];
          else throw err;
        }
        fileList.push(fileObject)
      });
      return fileList.filter((file) => typeof(file) == 'object');
    },
    handleFileClick(file){
      this.$emit('file-clicked',file)
    }

  },
  watch:{
    directoryPath(){
      if(this.directoryPath != 'C:\\') {
          //console.time('getDirectoyFiles')
          this.directoryFiles = this.getDirectoryFiles(this.directoryPath);
          //console.timeEnd('getDirectoyFiles')
          //this.directoryFiles.splice(0,0,'test')
        }
    }
  }
}
</script>

<style lang='sass'>

@import '../styles/fileExplorer.scss'
</style>
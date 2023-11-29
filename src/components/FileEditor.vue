<template>
  <div class="opened-file-editor" :data-fileIndex="fileIndex" v-if="openedFile == focusedFile">

    <div id="line-nums" class="line-nums"></div>

    <textarea 
      v-model="fileContent" 
      id="code-editor" 
      class="code-editor" 
      @keyup.enter="checkChange()" 
      @keyup.delete="checkChange()"
      @keydown.tab.prevent="null"
      @load="checkChange()"
      :spellcheck="false"
      >
    </textarea>

    <div class="extras">
      <div class="button-operations">
        <button class="save-button" @click="saveFile(openedFile)">Save File</button>
        <button class="terminal-button" @click="openCmd()">open terminal</button>
      </div>

      <div class="terminal" id="Terminal">
      
      </div>
    </div>



  </div>
</template>

<script>
import fs from 'fs'
import path from 'path';

export default {
  name:'FileEditor',
  components:[
  ],
  data() {
    return {
      fileContent:'',
      fileExtension:'',
      lineAmount: 0,
    }
  },
  computed:{
  },
  props:{
    openedFile:{
      type: Object,
      default: {}
    },
    focusedFile:{
      type:Object,
      default: {}
    },
    fileIndex:{
      type:String,
      default: ''
    }
  },
  created(){
  },
  mounted(){
    this.readAndSetFile(this.openedFile)
    this.allowTab()
    //this.checkChange()

    window.addEventListener('keydown', function (event) {
      if (event.ctrlKey && event.shiftKey && event.code === 'KeyU') {
          console.log(event);
      }
    });
  },
  methods:{
    readAndSetFile(file){

      fs.readFile(file.filePath,{encoding:'utf8'}, (err,data)=>{
        if(err){ 
          console.log(err)
          return
        };
        this.fileExtension = path.extname(file.filePath)
        data.replaceAll(' ','\n')
        this.fileContent = data

      });
    },
    checkChange(){
      /* let codeEditor = document.getElementById('code-editor');
      let count = codeEditor.value.split('\n')
      this.lineAmount = count.length
      this.lineNums() */
    },
    lineNums(){
      let container = document.getElementById('line-nums')
      container.innerHTML = ''
      for (let i = 0; i < this.lineAmount; i++) {
        const element = document.createElement('div');
        element.innerText = i+1;
        element.classList.add('line-number')
        container.appendChild(element)
      }
    },
    saveFile(file){
    
      fs.writeFileSync(file.filePath,this.fileContent, function (err) {
        if (err) throw err;
        console.log('It\'s saved!');
      })
      this.readAndSetFile(file)
    },
    openCmd(){
      //child_process.exec("start cmd.exe");
    },
    allowTab(){
      var inputField = document.getElementById('code-editor'); // get textarea object
 
      inputField.onkeydown = function(e) { // list for when any key is pressed

        if (e.key === 'Tab' && e.shiftKey) { // block to catch when tab key is pressed
          inputField.setRangeText(

            '',

            inputField.selectionStart-2,

            inputField.selectionStart,

            'end'

          )
          return false; //prevent default action
        }

        if (e.key === 'Tab') { // block to catch when tab key is pressed
          inputField.setRangeText(

            '  ',

            inputField.selectionStart,

            inputField.selectionStart,

            'end'

          )
          return false; //prevent default action
        }
      };
    },
  },
  watch:{
    openedFile(){
      //this.checkChange()
    }
  },
  updated() {
  },
}
</script>

<style lang='scss'>
@import '@/styles/fileEditor.scss'
</style>
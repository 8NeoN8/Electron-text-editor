<template>
  <div v-for="(file, index) in fileList" :key="index">
    <div class="file-item">
      <!-- Map the fileList to find child files for every directory and point to them for recursive display-->
      <!-- <div class="file-directory" v-if="file.isDirectory">
        {{ file.filename }}

        <ul class="directory-files" v-for="(cfile, index) in file.childFiles" :key="index">
          {{ file.cfile.filename }}
        </ul>      
      </div>

      <div class="file-raw" v-if="!file.isDirectory">

      </div> -->

    </div>
  </div>
</template>

<script>
export default {
  name:'FileViewer',
  components:{
  },
  props:{
    fileList:{
      type: Array,
      default: [],
    }
  },
  data() {
    return {
      formattdFileList: null
    }
  },
  computed:{
    tempFileList(){
      let tempFiles = this.fileList
      return tempFiles
    }
  },
  created(){
  },
  mounted(){
  },
  methods:{
    setSubDirectories(directoryArray){
      
    },

  },
  watch:{
    tempFileList(){
      /* let mutFileList = this.tempFileList
      this.formattdFileList = mutFileList.map((file) => {
        if(file.isDirectory){
          mutFileList.forEach(subfile => {
            if(subfile.filePath.includes(file.filename) && subfile.filePath != file.filePath){
              file.subDirectories.push(subfile)
            }
          });
        }
        return file
      })


      this.formattdFileList = this.formattdFileList.forEach(file => {
        
      });


      console.log("Formatted>>: ",this.formattdFileList); */

      console.log(this.tempFileList);

      
    }
  },
  updated(){
  }
}
</script>

<style lang='sass'>
</style>
<template>
  <div 
    class="file-node" 
    @click="fileNode.isDirectory ? toggleDirectory(fileNode) : handleFileClick(fileNode)" 
    :class="fileNode.isDirectory ? 'directory-node' : null"
  >

    {{ fileNode.filename }}

  </div>

  <div 
    v-if="fileNode.isDirectory && fileNode.showSubDirectories" 
    class="sub-directory-node"
  >

    <FileTree 
      v-for="(subFile, index) in fileNode.subDirectories" 
      :key="index" 
      :fileNode="subFile"
      @file-clicked="handleFileClick($event)"
    />

  </div>

</template>

<script>
export default {
  name:'FileTree',
  emits:['file-clicked'],
  components:{
  },
  props:{
    fileNode:{
      type: Object,
      default: null,
    }
  },
  data() {
    return {
      formattdFileList: null
    }
  },
  computed:{
    tempFileNode(){
      let tempFile = this.fileNode
      return tempFile
    }
  },
  created(){
  },
  mounted(){
  },
  methods:{
    toggleDirectory(file){
      file.showSubDirectories = !file.showSubDirectories
    },
    handleFileClick(file){
      this.$emit('file-clicked',file)
    }
  },
  watch:{
    tempFileNode(){
      //console.log(this.tempFile);
    }
  },
  updated(){
  }
}
</script>

<style lang='sass'>
@import '@/styles/fileTree.scss'
</style>
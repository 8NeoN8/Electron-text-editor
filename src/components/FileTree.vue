<template>
  <div class="file-node" @click="toggleDirectory(fileNode)">
    {{ fileNode.filename }}
  </div>

  <div v-if="fileNode.isDirectory" :class="fileNode.isDirectory ? 'sub-directory-file' : null">
    <FileTree v-for="(subFile, index) in fileNode.subDirectories" :key="index" :fileNode="subFile"/>
  </div>

</template>

<script>
export default {
  name:'FileTree',
  components:{
  },
  props:{
    fileNode:{
      type: Object,
      default: {},
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
      if(file.isDirectory) console.log('clicked a directory');
    }
  },
  watch:{
    tempFileNode(){
      console.log(this.tempFile);
    }
  },
  updated(){
  }
}
</script>

<style lang='sass'>
@import '@/styles/fileTree.scss'
</style>
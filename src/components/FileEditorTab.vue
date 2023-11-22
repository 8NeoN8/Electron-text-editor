<template>
  <div class="file-editor-tabs" >
    <template v-for="(file, index) in openedFiles" :key="index">
      <div class="tab"  @click="clickedTab($event,file)">
        <span class="file-type-icon">
          Txt
        </span>

        <span class="file-name">
          {{ file.filename }}
        </span>

        <span class="file-close-tab" @click="closeTab($event,file)">
          <i class="fa-solid fa-xmark fa-sm"></i>
        </span>

      </div>
    </template>
  </div>
</template>

<script>
export default {
  name:'FileEditorTab',
  components:{},
  emits:['clickedTab','closeTab'],
  data() {
    return {

    }
  },
  props:{
    openedFiles:{
      type:Object,
      default: {}
    },
    focusedFile:{
      type:Object,
      default:{}
    }
  },
  created(){},
  mounted(){
   },
  methods:{
    clickedTab(tab,file){
      let tabContent = {
        file: Object.assign(file),
        tab: Object.assign(tab.target.parentNode)
      }
      this.$emit('clickedTab',tabContent)

      if(file == this.focusedFile) return

      document.querySelectorAll('.tab').forEach(tabElement => {
        tabElement.classList.remove('active-tab')
      });

      tab.target.classList.add('active-tab')
    },
    closeTab(tab,file){
      let tabContent = {
        file: Object.assign(file),
        tab: Object.assign(tab.target.parentNode)
      }
      this.$emit('closeTab',tabContent)
    }
  },
  watch:{}
}
</script>

<style lang='scss'>
@import '@/styles/fileEditorTab.scss'
</style>
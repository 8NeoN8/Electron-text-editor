<template>

  <div id="title-bar" class="title-bar">
    <div class="title-icon-name">
      <div class="title-icon">
        <i class="fa-solid fa-shield-halved fa-lg"></i>
      </div>

      <div class="title-name">
        Servant of Shield
      </div>
    </div>

    <div class="title-filename">
      
      
      
    </div>

    <div class="window-controls">
      <div id="minimize-button" class="button">
        <i class="fa-solid fa-window-minimize"></i>
      </div>

      <div id="maximize-button" class="button">
        <i class="fa-solid fa-window-maximize"></i>
      </div>

      <div id="restore-button" class="button">
        <i class="fa-solid fa-window-restore"></i>
      </div>

      <div id="close-button" class="button">
        <i class="fa-solid fa-xmark fa-xl"></i>
      </div>
    </div>
  </div>

</template>

<script>

import { format } from 'date-fns'

export default {
  name:'TitleBar',
  components:{},
  data() {
    return {
      currentDate: format(new Date(), 'MM/dd/yy'),
      currentHourMinute: format(new Date(), 'hh:mm aaaa')
    }
  },
  computed:{
  },
  created(){
    setInterval(() => {
      this.currentHourMinute = format(new Date(), 'hh:mm aaaa')
    }, 60000);
  },
  mounted(){
    //this.test()
   },
  methods:{
    test(){
      console.log('Test Function')
    },
  },
  watch:{}
}
</script>

<style lang="sass">
@import '../styles/TitleBar.scss'
</style>
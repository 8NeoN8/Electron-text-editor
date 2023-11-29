<template>

  <TitleBar :directoryPath="directoryPath"/>

  <router-view id="main" @clicked="test($event)"/>

</template>

<script>
//Stop error resizeObserver
/* const debounce = (delay) => {
  let tid;
  return function () {
    const ctx = self;
    tid && clearTimeout(tid);
    tid = setTimeout((callback) => {
      callback.apply(ctx, args);
    }, delay);
  };
};

const _ = (window).ResizeObserver;
(window).ResizeObserver = class ResizeObserver extends _ {
  constructor(callback) {
    callback = debounce (callback, 20);
    super(callback);
  }
}; */


import TitleBar from "@/components/TitleBar.vue";


//@@@@@@@@ Need to reorganize this code @@@@@@@@

//detect when in full screen
const remote = require("@electron/remote");
window.addEventListener('resize', event => {
  if (remote.getCurrentWindow().isFullScreen()) {
      //console.log('full');
    }
})

//If in full screen, when Esc is pressed exit full screen
document.addEventListener("keydown", event => {
  switch (event.key) {
    case "Escape":
      if (remote.getCurrentWindow().isFullScreen()) {
        remote.getCurrentWindow().setFullScreen(false);
      }
      break;
    }
});

const win = remote.getCurrentWindow(); /* Note this is different to the html global `window` variable */

// When document has loaded, initialise
document.onreadystatechange = (event) => {
    if (document.readyState == "complete") {
        handleWindowControls(); 
    }
};

window.onbeforeunload = (event) => {
    /* If window is reloaded, remove win event listeners
    (DOM element listeners get auto garbage collected but not
    Electron win listeners as the win is not dereferenced unless closed) */
    win.removeAllListeners();
    //console.log('removing listeners');
}

function handleWindowControls() {
    // Make minimise/maximise/restore/close buttons work when they are clicked
    document.getElementById('minimize-button').addEventListener("click", event => {
        win.minimize();
        //console.log('minimize');
    });

    document.getElementById('maximize-button').addEventListener("click", event => {
        win.maximize();
        //console.log('maximize');
    });

    document.getElementById('restore-button').addEventListener("click", event => {
        win.unmaximize();
        //console.log('unmaximize');
    });

    document.getElementById('close-button').addEventListener("click", event => {
      //console.log('close');
      win.close()
    });

    // Toggle maximise/restore buttons when maximisation/unmaximisation occurs
    toggleMaxRestoreButtons();
    win.on('maximize', toggleMaxRestoreButtons);
    win.on('unmaximize', toggleMaxRestoreButtons);

    function toggleMaxRestoreButtons() {
        if (win.isMaximized()) {
            document.body.classList.add('maximized');
        } else {
            document.body.classList.remove('maximized');
        }
    }
}

//@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

export default {
  components:{
    TitleBar,
  },
  data() {
    return {
      directoryPath: null
    }
  },
  methods:{
  }
}

</script>



<style lang="scss">

@import './styles/global';

</style>

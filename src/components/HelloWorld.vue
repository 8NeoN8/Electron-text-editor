<template>
  <div id="editor" class="hello" contenteditable="true" @keyup.enter="test()">
    this is a test
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  watch:{

  },
  methods: {
    test(){
      let editor = document.getElementById('editor')
      let lines = Array.from(editor.querySelectorAll("div"))
      lines.forEach(line => {
        let lineText = line.innerText
        let characters = lineText.split('')
        console.log(characters[0]);
        if(characters[0] == '#' && characters[1] != '#'){

          characters = characters.splice(1)
          let clone = document.createElement('h1')
          clone.innerText = characters.join('')
          line.replaceWith(clone)
        }
      });
    },
    changeHTML(el){
      let ogText = el.innerText
      el.innerHTML = `<div>${ogText}<div/>`
    },
  },
  mounted() {
    let editor = document.getElementById('editor')
    this.changeHTML(editor)
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.hello{
  background-color: red;
}

*{
  text-align: left;
}
/* h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
} */
</style>

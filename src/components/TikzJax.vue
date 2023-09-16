<template>
  <div id="tikzjax_box"> 
    <!-- <ul class="infinite-list" v-infinite-scroll="load" style="overflow:auto">
    <li v-for="(item,index) in items" :key="index" class="infinite-list-item">{{ i }}</li>
    </ul> -->
  </div>

</template>

<script>

export default {
  props: ['code'],
  data(){
    return{
      isDrwing:false,
      startPoint:null,
      endPoint:null,
      selectShape:this,
      // count:0
    }
  },
  watch: {
    code: function(newVal) {
      let tikzjax_box = document.getElementById("tikzjax_box")
      while (tikzjax_box.hasChildNodes()) {
        tikzjax_box.removeChild(tikzjax_box.firstChild);
      }
      let script_tikzjax = document.createElement("script")
      script_tikzjax.type = "text/tikz"
      script_tikzjax.innerHTML = '\\begin{tikzpicture}\n'+"\\draw[black][line width=0.01] (0,0) rectangle (18.25,14.4);\n "+newVal+'\\end{tikzpicture}'
      //18.25 14.4
      tikzjax_box.appendChild(script_tikzjax)
      window.onload()
    }
  },
  mounted() {
      let tikzjax_box = document.getElementById("tikzjax_box")
      let script_tikzjax = document.createElement("script")
      script_tikzjax.type = "text/tikz"
      script_tikzjax.innerHTML = '\\begin{tikzpicture}\n'+'\\draw[black][line width=0.01] (0,0) rectangle (18.25,14.4);\n \\end{tikzpicture}'
      //18.25 14.4
      tikzjax_box.appendChild(script_tikzjax)
      window.onload()
  },
  // methods: {
  //     load () {
  //       this.count += 2
  //     }
  //   }
};
</script>

<style scoped>
</style>
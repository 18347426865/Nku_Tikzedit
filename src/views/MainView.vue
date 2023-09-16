<template>
  <el-container>
    <el-header>
      <HeaderComponent />
    </el-header>
    <!-- <p>{{ shape }}:{{ isDrawingArrow }}</p>
    <p>{{ lineWidth }}</p> -->
    <el-main class="toolbar-container">
      <ToolBar v-on:ShapeChange="ShapeChange" v-on:LineWidth="changeLineWidth" v-on:LineColor="changeLineColor" v-on:LineStyle="changeLineStyle" ref="toolbar"/>
      <div class="codeedit-button-container">
        <el-button round @click="doUpdate"> 运行 </el-button>
        <el-button round @click="resetInput"> 重置 </el-button>
      </div>
    </el-main>
    <el-main class="func-container">
      <div id="codeedit-container" class="codeedit-container">
        <el-input type="textarea" :row="10" autosize v-model="input"  resize="none"></el-input>
        <ul class="infinite-list" v-infinite-scroll="load" style="overflow:auto">
        <li v-for="(item,index) in items" :key="index" class="infinite-list-item">{{ i }}</li>
        </ul>
        <!-- <div class="codeedit-button-container">
          <el-button type="primary" @click="doUpdate">运行</el-button>
          <el-button @click="resetInput">重置</el-button>
        </div> -->
      </div>
      <div id= "tikzjax-container" class="tikzjax-container" 
      @mousedown="HandleMouseDown" 
      @mousemove="HandleMouseMove" 
      @mouseup="HandleMouseUp">
        <TikzJax :code="code"/>
      </div>
    </el-main>
    <el-footer>
      <FooterComponent />
    </el-footer>
  </el-container>
</template>
  
<script>
import TikzJax from '../components/TikzJax.vue'
import HeaderComponent from '../components/HeaderComponent.vue'
import FooterComponent from '../components/FooterComponent.vue'
import ToolBar from '../components/ToolBar.vue'

export default {
  name: 'MainView',
  components: {
    TikzJax,
    HeaderComponent,
    FooterComponent,
    ToolBar,
  },
  data() {
    return {
      begin:'\\begin{tikzpicture}\n ',
      end:'\\end{tikzpicture}',
      input: '',
      code: '',
      shape:'',
      isDrawing:false,
      startPoint:null,
      endPoint:null,
      list_code:"\\draw[black][line width=1] (0,0) rectangle (20,10);\n ",
      isDrawingArrow:false,
      arrowPoints:[],
      lineWidth:2,
      lineColor:'black',
      lineStyle:'',
      count:0
    }
  },
  mounted(){
    this.tikz = document.getElementById('tikzjax-container');
  },
  methods: {
    load () {
        this.count += 2
      },
    resetInput() {
      this.input = "";
      this.list_code="\\draw[black][line width=1] (0,0) rectangle (20,10);\n ";
      this.code="";
    },
    doUpdate() {
      this.code = this.input
    },
    changeLineStyle(LineStyle)
    {
      this.lineStyle=LineStyle;
    },
    changeLineWidth(LineWidth){
      this.lineWidth=LineWidth;
    },
    changeLineColor(LineColor){
      this.lineColor=LineColor;
    },
    ShapeChange(selectShape){
      this.shape=selectShape;
    },
    startArrowDrawing(DrawArrow){
      this.isDrawingArrow = DrawArrow;
      this.arrowPoints = [];
    },
    handleCanvasMouseClick(e) {
      if (this.isDrawingArrow && e.button === 0) {
        const x = e.clientX;
        const y = 38-e.clientY;
        this.arrowPoints.push({ x, y });
      }
    },
    handleCanvasContextMenu(e) {
      e.preventDefault();
      if (this.isDrawingArrow && this.arrowPoints.length > 1) {
        this.isDrawingArrow = false;
        this.drawArrow();
      }
    },
    drawArrow() {
      const tikzPoints = this.arrowPoints.map(point => `(${point.x / 10},${point.y / 10})`).join(' -- ');
      this.list_code += `\\draw[${this.lineColor}][line width=${this.lineWidth}] ${tikzPoints};`;
    },

    HandleMouseDown(event){
      this.isDrawing=true;
      const x = event.offsetX/38;
      const y = (547-event.offsetY)/38;
      this.startPoint = { x, y };
    },
    HandleMouseMove(event){
      if (!this.isDrawing) return;
      const x = event.offsetX/38;
      const y = (547-event.offsetY)/38;
      this.endPoint = {x,y};
    },
    HandleMouseUp(){
      this.isDrawing=false;
      if(this.shape=='rectangle')
      {
        this.input+=`\\draw[${this.lineColor}][line width=${this.lineWidth}] (${this.startPoint.x},${this.startPoint.y}) rectangle (${this.endPoint.x},${this.endPoint.y});\n `;
        this.doUpdate();
      }else if(this.shape=='circle')
      {
        const radius=Number(Math.sqrt(Math.pow(this.endPoint.x-this.startPoint.x,2)+Math.pow(this.endPoint.y-this.startPoint.y,2)));
        this.input += `\\draw[${this.lineColor}] [line width=${this.lineWidth}](${this.startPoint.x},${this.startPoint.y}) circle (${radius});\n `;
        this.doUpdate();
        
      }else if(this.shape=='point')
      {
        this.input += `\\fill [${this.lineColor}][line width=${this.lineWidth}](${this.startPoint.x},${this.startPoint.y}) circle [radius=2pt];\n `;
        this.doUpdate();
      }else if(this.shape=='node')
      {
        this.input+=`\\node at (${this.startPoint.x},${this.startPoint.y}) {};\n `;
      }else if(this.shape=='arrow')
      {
  
        this.input+=`\\draw[->][${this.lineColor}][line width=${this.lineWidth}][${this.lineStyle}] (${this.startPoint.x},${this.startPoint.y}) -- (${this.endPoint.x},${this.endPoint.y});\n `;
        this.doUpdate();
      }
    }
  }
}
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.el-container {
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
}

.el-header {
  padding: 0;
  /* background-color: red; */
}

.el-footer {
  padding: 0;
  /* background-color: green; */
}

.el-main {
  display: flex;
  flex-direction: column; 
  align-items: center; 
  padding: 0;
}

.toolbar-container {
  height: 20%;
  width: 100%;
  background-color: rgb(245, 245, 242);
}

.func-container {
  height: 90%;
  width: 100%;
  display: flex;
  flex-direction: row;  
  align-items: center;
  background-color: rgb(245, 245, 242);
}

.codeedit-container{
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: rgb(245, 245, 242);
  height: 100%;
  width: 50%;
  overflow-y: scroll; 
}

.codeedit-button-container{
  justify-content: space-between;
  height: 10%;
  padding: 0.75%;
  margin-right: 15px
}

.tikzjax-container{
  /* background-color: pink; */
  height: 100%;
  width: 50%;
}


</style>
  
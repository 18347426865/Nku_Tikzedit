<template>
    <div>
        <el-button  plain @click="checkshapeChange('rectangle')">rectangle</el-button>
        <el-button  plain @click="shape=checkshapeChange('circle')">circle</el-button>
        <el-button  plain @click="shape=checkshapeChange('point')">point</el-button>
        <el-button  plain @click="shape=checkshapeChange('arrow')">arrow</el-button>
        <el-button  plain @click="shape=checkshapeChange('node')">Node</el-button>
        <div class = "one">
            Width:
            <select class = linewidth v-model="lineWidth">
                <option id=1 value="2">2px</option>
                <option id=2 value="3">3px</option>
                <option id=3 value="4">4px</option>
                <option id=4 value="5">5px</option>
                <option id=5 value="6">6px</option>
            </select>
            <!-- <el-button plain @click="changeLineWidth()">修改</el-button> -->
            <el-button class = linewidth icon="el-icon-check" circle @click="changeLineWidth()"></el-button>
        </div>
        <div class = "two">
            Color:
            <select class = linewidth v-model="lineColor">
                <option id=1 value="black">black</option>
                <option id=2 value="yellow">yellow</option>
                <option id=3 value="blue">blue</option>
                <option id=4 value="green">green</option>
            </select>
            <!-- <button @click="changeLineColor()">修改</button> -->
            <el-button class = linewidth linewidth icon="el-icon-check" circle @click="changeLineColor()"></el-button>
        </div>
        <div class = "three">
            Line Style:
            <select class = linewidth v-model="lineStyle">
                <option id=1 value=" ">solid line</option>
                <option id=2 value="densely dotted">tight dotted line</option>
                <option id=3 value="dotted">dotted line</option>
            </select>
            <!-- <button @click="changeLineColor()">修改</button> -->
            <el-button class = linewidth linewidth icon="el-icon-check" circle @click="changeLineStyle()"></el-button>
        </div>
    </div>
</template>

<script>
// import { Button } from 'element-ui';
export default {
    data() {
        return {
            shape: "",
            isDrawingArrow: false,
            lineColor: 'black',
            lineWidth: 2,
            lineStyle:''
        };
    },
    methods: {
        changeLineStyle(){
            alert("线条"+this.lineStyle)
            this.$emit('LineStyle',this.lineStyle);
        },
        changeLineWidth() {
            this.$emit('LineWidth', this.lineWidth);
        },
        changeLineColor() {
            this.$emit('LineColor', this.lineColor);
        },
        DrawArrow() {
            this.isDrawingArrow = true;
            this.$emit('DrawArrow', this.isDrawingArrow);
        },
        checkshapeChange(value) {
            this.shape = value;
            this.$emit('ShapeChange', this.shape);
        },
        getShape() {
            return this.shape;
        }
    },
    // components: { Button }
};
</script>

<style scoped>
div{float: left;}
.el-button {
  background-color: white;
  font-size: 15px;
  margin-top:20px;
  margin-left:20px
}
.one{
    font-family:'Times New Roman', Times, serif;
    font-size: 20px;
    color: #000;
    margin-top:7px;
    margin-left:20px
    /* float: left; */
    /* margin-right: 20px; */
}

.two{
    font-family: 'Times New Roman', Times, serif;
    font-size: 20px;
    color: #000;
    margin-top:7px;
    margin-left:20px
    /* float: right; */
    /* margin-left: 20px; */
}

.three{
    font-family: 'Times New Roman', Times, serif;
    font-size: 20px;
    color: #000;
    margin-top:7px;
    margin-left:20px;
}
.linewidth{
    font-family: 'Times New Roman', Times, serif;
    font-size: 15px;
    margin-left: 10px;
    padding: 5px;
}

</style>


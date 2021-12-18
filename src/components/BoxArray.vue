

<template>
    <div id="arrayBox"></div>
</template>

<script>
import $ from "jquery";
import * as joint from "jointjs"
 export default {
    props: {
      array:{
        type: Array,
        default: []
      }
    },
    mounted() { 
      this.drawBoxes()
    },
    methods:{
      drawBoxes() {
        let leftArray = Object.entries(this.array.leftArray).map(([key, value]) => ({key,value}));
        let rightArray = Object.entries(this.array.rightArray).map(([key, value]) => ({key,value}));
        let maxArrayLength = leftArray.length > rightArray.length ? leftArray.length : rightArray.length;
        let width = 0;
        if(window.innerWidth > 800){
          width = window.innerWidth * 0.4;
        }
        else{
          width = window.innerWidth * 0.8;
        }    
        let boxwidth = width / 5;
        let boxheight = boxwidth / 3;
        let height = maxArrayLength * (boxheight + 20)
        let namespace = joint.shapes; 

        let graph = new joint.dia.Graph({}, { cellNamespace: namespace });
        var paper = new joint.dia.Paper({
            el: document.getElementById('arrayBox'),
            width: width,
            height: height,
            model: graph,
            defaultConnectionPoint: { name: 'boundary' },
            defaultConnector: { name: 'smooth' },
            interactive: { linkMove: false },
            frozen: true,
        });

        paper.on('element:label:pointerdown', function (_view, evt) {
          evt.stopPropagation();
        });

        paper.on('cell:pointerdown blank:pointerdown', function () {
          if (window.getSelection) {
            window.getSelection().removeAllRanges();
          } else if (document.selection) {
            document.selection.empty();
          }
        });
        
        function createBox(x, y, label) {
          var circle = new joint.shapes.standard.Rectangle({
            position: { x: x, y: y },
            size: { width: boxwidth, height: boxheight },
            attrs: {
              label: {
                text: label,
                fontWeight: 'bold',
                cursor: 'text',
                style: {
                  userSelect: 'text',
                },
              },
              body: {
                strokeWidth: 1,
              },
            },
          });
          return circle.addTo(graph);
        }

        function link(source, target, vertices) {
          var link = new joint.shapes.standard.Link({
            source: { id: source.id },
            target: { id: target.id },
            attrs: {
              line: {
                strokeWidth: 2,
              },
            },
            vertices: vertices || [],
          });
          return link.addTo(graph);
        }

        let leftarraybox = [];
        let rightArraybox = [];
        leftArray.map((item, index)=>{
          let Box = createBox(10, 10 + index * 50, item.value);
          leftarraybox.push(Box)
        })
        
        rightArray.map((item, index)=>{
          let Box = createBox(width - 10 -boxwidth , 10 + index * 50, item.value);
          rightArraybox.push(Box)
        })
        
        for(let i = 0; i < leftArray.length; i++) {
          for(let j = 0; j<rightArray.length; j++){
            if(leftArray[i].key=== rightArray[j].key){
              link(leftarraybox[i], rightArraybox[j])
            }
          }
        }
        paper.unfreeze();
      }
    },
    created() {
      window.addEventListener("resize", this.drawBoxes);
    },
    destroyed() {
     window.removeEventListener("resize", this.drawBoxes);
    }
  }
</script>
<style>
  #arrayBox{
    margin:auto
  }
</style>
<template>
  <div id="arrayBox"></div>
</template>

<script>
import * as joint from "jointjs";
export default {
  props: {
    array: {
      type: Array,
      default: [],
    },
    leftArray: {
      type: Array,
      default: [],
    },
    rightArray: {
      type: Array,
      default: [],
    },
  },
  mounted() {
    this.drawBoxes();
  },
  created() {
    window.addEventListener("resize", this.drawBoxes);
  },
  unmounted() {
    window.removeEventListener("resize", this.drawBoxes);
  },
  methods: {
    drawBoxes() {
      let linkArray = this.array;
      let maxArrayLength =
        this.leftArray.length > this.rightArray.length
          ? this.leftArray.length
          : this.rightArray.length;
      let width = 0;
      if (window.innerWidth > 800) {
        width = window.innerWidth * 0.4;
      } else {
        width = window.innerWidth * 0.8;
      }

      let boxwidth = width / 5;
      let boxheight = boxwidth / 3;
      let height = maxArrayLength * (boxheight + 20);
      let namespace = joint.shapes;

      let graph = new joint.dia.Graph({}, { cellNamespace: namespace });
      var paper = new joint.dia.Paper({
        el: document.getElementById("arrayBox"),
        width: width,
        height: height,
        model: graph,
        defaultConnectionPoint: { name: "boundary" },
        defaultConnector: { name: "smooth" },
        interactive: { linkMove: false },
        frozen: true,
      });

      paper.on("element:label:pointerdown", function (_view, evt) {
        evt.stopPropagation();
      });

      paper.on("cell:pointerdown blank:pointerdown", function () {
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
              fontWeight: "bold",
              cursor: "text",
              style: {
                userSelect: "text",
              },
            },
            body: {
              strokeWidth: 1,
            },
          },
        });
        return circle.addTo(graph);
      }

      function link(source, target, priority, vertices) {
        var link = new joint.shapes.standard.Link({
          source: { id: source.id },
          target: { id: target.id },
          attrs: {
            line: {
              strokeWidth: priority || 1,
            },
          },
          vertices: vertices || [],
        });
        return link.addTo(graph);
      }

      let leftarraybox = [];
      let rightArraybox = [];

      this.leftArray.map((item, index) => {
        let Box = createBox(10, 10 + index * 50, item.label);
        leftarraybox.push(Box);
      });

      this.rightArray.map((item, index) => {
        let Box = createBox(width - 10 - boxwidth, 10 + index * 50, item.label);
        rightArraybox.push(Box);
      });

      const leftArrayId = this.leftArray
        .map((item) => item.leftID)
        .filter((v, i, a) => a.indexOf(v) === i);

      const rightArrayId = this.rightArray
        .map((item) => item.rightID)
        .filter((v, i, a) => a.indexOf(v) === i);

      linkArray.map((item) => {
        link(
          leftarraybox[leftArrayId.indexOf(item.leftID)],
          rightArraybox[rightArrayId.indexOf(item.rightID)],
          item.priority
        );
      });
      paper.unfreeze();
    },
  },
};
</script>
<style>
#arrayBox {
  margin: auto;
}
</style>

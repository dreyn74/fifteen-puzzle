<template>
  <v-group ref="group" :config="{ draggable: true, x: piece.x, y: piece.y }">
    <v-rect
      :config="{
        rotation: piece.rotation,
        id: piece.id,
        width: size.height * 0.1618,
        height: size.height * 0.1618,
        fill: '#89b717',
        opacity: 0.8,
        cornerRadius: 5,
        scaleX: dragPieceId === piece.id ? piece.scale * 1.2 : piece.scale,
        scaleY: dragPieceId === piece.id ? piece.scale * 1.2 : piece.scale,
        shadowColor: 'black',
        shadowBlur: 10,
        shadowOffsetX: dragPieceId === piece.id ? 15 : 5,
        shadowOffsetY: dragPieceId === piece.id ? 15 : 5,
        shadowOpacity: 0.6,
      }"
    ></v-rect>
    <v-text :config="configText"></v-text>
  </v-group>
</template>

<script>
export default {
  name: "Piece",
  props: ["piece", "grid", "size"],
  data() {
    return {
      dragPieceId: null,
    };
  },
  computed: {
    configText() {
      return {
        name: this.piece.id + "text",
        fontSize: 56,
        x: 17,
        y: 27,
        text: this.piece.id + 1,
        fill: this.piece.isDragging ? "red" : "blue",
        shadowBlur: 5,
      };
    },
  },
  methods: {
    handleDragstart(e) {
      let myGrid = this.grid;
      // save drag element:
      this.dragPieceId = e.piece.id();
      // move current element to the top:
      const item = myGrid.find((i) => i.id === this.dragPieceId);
      const index = myGrid.indexOf(item);
      myGrid.splice(index, 1);
      myGrid.push(item);
      this.$emit("update-grid", myGrid);
    },
    handleDragend(e) {
      this.dragPieceId = null;
    },
  },
};
</script>
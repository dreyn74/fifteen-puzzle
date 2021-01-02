<template>
  <v-stage
    ref="stage"
    :config="configKonva"
    @dragstart="handleDragstart"
    @dragend="handleDragend"
  >
    <v-layer ref="baseLayer" :config="configBaseLayer">
      <v-rect ref="board" :config="configBoard" />
      <Piece
        v-for="piece in grid"
        :key="piece.id"
        :isDragging="false"
        :piece="piece"
        :size="size"
        :grid="grid"
      />
    </v-layer>
  </v-stage>
</template>

<script>
import Piece from "./Piece";
const width = window.innerWidth;
const height = window.innerHeight;

export default {
  name: "Board",
  props: ["puzzle"],
  components: { Piece },
  data() {
    return {
      grid: [],
      parity: 0,
      isSolvable: false,
      gridWidth: 4,
      row: 0,
      blankRow: 0,
      configKonva: {
        width: width,
        height: height,
      },
      configBaseLayer: {
        listening: true,
      },
    };
  },
  computed: {
    size() {
      return { width, height };
    },
    boardLocation() {
      let x = this.size.width * 0.15;
      let y = this.size.height * 0.15;
      return { x, y };
    },
    /**/
    boardWidth() {
      return this.size.width * 0.8;
    },
    boardHeight() {
      return this.size.height * 0.8;
    },
    configBoard() {
      return {
        cornerRadius: 15,
        fill: "lavender",
        x: this.boardLocation.x, //100,
        y: this.boardLocation.y, //100,
        width: this.boardHeight, //500,
        height: this.boardHeight, //500,
      };
    },
  },

  methods: {
    handleDragend() {
      return;
    },
    handleDragstart() {
      return;
    },
    updateGrid(grid) {
      this.grid = grid;
    },
  },
  mounted() {
    for (let n = 0; n < 15; n++) {
      this.grid.push({
        id: n, // Math.round(Math.random() * 10000).toString(),
        x: Math.random() * width,
        y: Math.random() * height,
        // rotation: Math.random() * 180,
        scale: 1, //Math.random(),
      });
    }
  },
};
</script>
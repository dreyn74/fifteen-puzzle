<template>
  <div id="app">
    <div class="header">
      <div class="header-item title" @click="onPuzzleBoardInit">New Game</div>
      <div class="header-item spacer"></div>
      <div @click="onMeowClick" class="header-item meow">😺MEOW!</div>
      <div class="header-item spacer"></div>

      <div class="header-item panel">
        <template v-if="isGoal">finish!!</template>
        <select v-model="difficulty">
          <option>Easy</option>
          <option>Normal</option>
          <option>Difficult</option>
        </select>
      </div>
      <div v-if="show">Moves: {{ moves }}</div>
    </div>
    <div class="container">
      <puzzle-board
        v-if="show"
        :autoResize="autoResize"
        :showNumber="showNumber"
        :cols="dimensions.x"
        :rows="dimensions.y"
        :src="src"
        :sources="sources"
        :animation="animation"
        :width="width"
        :height="height"
        @init="onPuzzleBoardInit"
        @start="onPuzzleBoardStart"
        @change="onPuzzleBoardChange"
        @finish="onPuzzleBoardFinish"
      />
    </div>
    <div class="repo" @click="onTitleClick">
      made by
      <a
        target="_blank"
        rel="noopener noreferrer"
        href="https://github.com/meganetaaan/vue-8-puzzle"
        >vue-8-puzzle</a
      >
    </div>
  </div>
</template>

<script>
// import Board from './components/Board'
import PuzzleBoard from "./components/PuzzleBoard.vue";

const SOURCEPATHS = {
  Dog: {
    //src: dogjpg,
  },
  Cat: {
    sources: [
      {
        src: "https://images.dog.ceo/breeds/chihuahua/n02085620_3407.jpg", // catwebm,
        type: "video/webm",
      },
      /*
      {
        src: catmp4,
        type: "video/mp4",
      },
      */
    ],
  },
  Penguin: {
    sources: [
      {
        //src: penguinwebm,
        type: "video/webm",
      },
      {
        //src: penguinmp4,
        type: "video/mp4",
      },
    ],
  },
  Giphy: {},
};
const getGiphySrc = async function () {
  const res = await fetch(
    "//api.giphy.com/v1/gifs/random?api_key=nZATgU4T4iauIJqCFu8rFvoCxCqBRwMt&rating=g&tag=cat"
  );
  const data = await res.json();
  const url = data.data.image_mp4_url;
  return [
    {
      src: url,
      type: "video/mp4",
    },
  ];
};
const DIMENSIONS = {
  Easy: { x: 3, y: 3 },
  Normal: { x: 4, y: 4 },
  Difficult: { x: 5, y: 5 },
};
export default {
  components: { PuzzleBoard },
  data() {
    return {
      show: true,
      moves: 0,
      videoTitle: "Cat",
      difficulty: "Normal",
      distance: null,
      isGoal: false,
      autoResize: true,
      width: 300,
      height: 300,
      showNumber: true,
      animation: true,
      sources: SOURCEPATHS["Giphy"].sources,
    };
  },
  created() {},
  computed: {
    src() {
      return SOURCEPATHS[this.videoTitle].src;
    },
    dimensions() {
      return DIMENSIONS[this.difficulty];
    },
  },
  watch: {
    isGoal(isGoal) {
      if (isGoal) {
        /*
        this.$confetti.start({
          shape: "rect",
        });
      } else {
        this.$confetti.stop(); 
      */
      }
    },
  },
  methods: {
    onTitleClick() {
      window.open("https://github.com/meganetaaan/vue-8-puzzle");
    },
    onMeowClick() {
      getGiphySrc().then((data) => {
        this.sources = data;
      });
    },
    onPuzzleBoardInit() {
      // console.log("init");
      getGiphySrc().then((data) => {
        this.sources = data;
      });
      this.isGoal = false;
    },
    onPuzzleBoardStart() {
      // console.log("start");
      this.moves = 0;
    },
    onPuzzleBoardFinish() {
      // console.log("finish");
      this.isGoal = true;
    },
    onPuzzleBoardChange: function (payload) {
      // console.log("change");
      this.distance = payload.distance;
      if (!this.isGoal) this.moves++;
    },
  },
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  background-color: #fafafa;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.header {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  z-index: 2;
  font-size: 1.4em;
  position: absolute;
  width: 100%;
  height: 60px;
  color: #fafafa;
  background-color: #e91e63;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
}
.header-item {
  flex-grow: 0;
}
.title {
  width: 20%;
  padding: 0 5px;
  font-weight: bold;
  font-size: 0.8em;
  cursor: pointer;
}
.spacer {
  flex-grow: 1;
}
.meow {
  color: white;
  cursor: pointer;
  font-weight: bold;
  font-size: 1.2em;
}
.panel {
  width: 20%;
}
.container {
  position: absolute;
  top: calc(50% + 30px);
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.16), 0 2px 4px rgba(0, 0, 0, 0.23);
  width: 90%;
  height: calc(90% - 60px);
  margin: 0px;
  padding: 0px;
  z-index: 1;
  background-color: #DDD;
  max-width: 600px;
  max-height: 600px;
}
.repo {
  position: absolute;
  bottom: 5px;
  right: 5px;
}
</style>
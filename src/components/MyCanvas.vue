<template>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Merriweather:ital,wght@1,700&family=Shizuru&display=swap"
    rel="stylesheet"
  />
  <div class="center">
    <div class="info">
      {{ level }}. Your task is to add two numbers to be equal to
      <span id="bigger">{{ toFind }}</span> .
    </div>
    <div class="info center">
      <div id="fact">
        {{ description }}
      </div>
    </div>
    <div ref="container" class="wrapper"></div>
  </div>
</template>

<script>
import Konva from "konva";
export default {
  data() {
    return {
      stage: null,
      layer: null,
      cosntainer: null,
      windowWidth: 100,
      windowHeight: 100,
      sum: [],
      solved: 0,
      toFind: 4,
      numOfObjects: 10,
      level: 0,
      fetchedLevels: [],
      finishedLevels: [],
      description: "",
    };
  },
  mounted() {
    this.fetchJson();
    this.stage = new Konva.Stage({
      container: this.$refs.container,
      width: this.windowWidth,
      height: this.windowHeight,
    });
    this.layer = new Konva.Layer();
    this.stage.add(this.layer);
    this.setCanvasResponsive();
    window.addEventListener("resize", this.setCanvasResponsive);
    this.sumContainer();
    this.stage.on("dragend", (event) => this.controlSum(event));
  },
  methods: {
    setCanvasResponsive() {
      this.windowHeight = window.innerHeight;
      this.windowWidth = window.innerWidth;
      this.stage.width(this.windowWidth);
      this.stage.height(this.windowHeight * 0.5);
      if (this.container) {
        this.container.width(this.windowWidth * 0.6);
        this.container.height(this.windowHeight * 0.5);
        this.container.x(this.windowWidth * 0.75);
      }
    },
    sumContainer() {
      this.container = new Konva.Rect({
        x: this.windowWidth * 0.6,
        y: 0,
        width: this.windowWidth * 0.6,
        height: this.windowHeight * 0.5,
        fill: "rgb(183, 0, 255,0.05)",
        stroke: "black",
        cornerRadius: [70, 0, 0, 70],
        shadowBlur: 10,
      });
      this.layer.add(this.container);
    },
    generateSetOfNumbers() {
      for (let i = 0; i < this.numOfObjects; i++) {
        const rand = Math.trunc(Math.random() * (this.toFind / 2)) + 1;
        this.generateNumber(rand);
        this.generateNumber(this.toFind - rand);
      }
    },
    generateNumber(innerNum) {
      const [r, g, b] = [
        Math.random() * 200 + 25,
        Math.random() * 200 + 25,
        Math.random() * 200 + 25,
      ];
      const num = new Konva.Label({
        x: this.windowWidth * (Math.random() * 0.3) + 40,
        y: this.windowHeight * (Math.random() * 0.38) + 40,
        draggable: true,
        hitStrokeWidth: 20,
      });
      num.add(
        new Konva.Tag({
          fill: `rgb(${r},${g},${b})`,
          stroke: "black",
        }).cornerRadius(50)
      );
      num.add(
        new Konva.Text({
          text: innerNum,
          fontSize: 30,
          fill: "black",
          padding: 15,
          align: "center",
        })
      );
      this.layer.add(num);
    },
    controlSum(event) {
      if (event.target.attrs.x > this.windowWidth * 0.6) {
        if (this.sum[0]) {
          const add =
            parseInt(this.sum[0].children[1].attrs.text) +
            parseInt(event.target.children[1].attrs.text);
          this.sum.push(event.target);
          if (add === this.toFind) {
            this.sum[0].destroy();
            this.sum[1].destroy();
            this.layer.batchDraw();
            this.sum = [];
            this.container.fill("green");
            this.isSolved();
          } else {
            this.sum[0].attrs.x = 0;
            this.sum[0].attrs.y = 0;
            this.sum[1].attrs.x = 0;
            this.sum[1].attrs.y = 0;
            this.sum[0].move({
              x: this.windowWidth * (Math.random() * 0.4) + 100,
              y: this.windowWidth * (Math.random() * 0.2) + 100,
            });
            this.sum[1].move({
              x: this.windowWidth * (Math.random() * 0.4) + 100,
              y: this.windowWidth * (Math.random() * 0.2) + 100,
            });
            this.container.fill("red");
            this.layer.batchDraw();
            this.sum = [];
          }
        } else {
          this.sum.push(event.target);
        }
        setTimeout(() => {
          this.container.fill("rgb(183, 0, 255,0.05)");
        }, 2000);
      }
    },
    isSolved() {
      this.solved++;
      if (this.solved >= this.numOfObjects) {
        this.searchLevel();
      }
    },
    fetchJson() {
      if (localStorage.getItem("levels")) {
        this.finishedLevels = JSON.parse(localStorage.getItem("levels"));
      }
      fetch("../levels.json")
        .then((res) => res.json())
        .then((data) => {
          this.fetchedLevels.push(...data);
          this.searchLevel();
        });
    },
    searchLevel() {
      if (this.finishedLevels.length >= 10) {
        this.finishedLevels = [];
      }
      let rand = Math.trunc(Math.random() * 10);
      if (
        this.finishedLevels.find(
          (el) => el.name === this.fetchedLevels[rand].name
        )
      ) {
        this.searchLevel();
      } else {
        this.finishedLevels.push(this.fetchedLevels[rand]);
        localStorage.setItem("levels", JSON.stringify(this.finishedLevels));
        this.level = this.fetchedLevels[rand].name;
        this.toFind = this.fetchedLevels[rand].sum;
        this.description = this.fetchedLevels[rand].description;
        this.generateSetOfNumbers();
      }
    },
  },
};
</script>

<style scoped>
.center {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.wrapper {
  background-color: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.459);
  border-radius: 0 0 5rem 5rem;
}
.info {
  width: 98.7%;
  padding: 1rem;
  margin-bottom: 1rem;
  color: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.247);
  text-align: center;
  font-family: "Merriweather", serif;
  font-size: 1.4em;
  background: linear-gradient(to right, white, rgb(183, 0, 255), white);
}
#bigger {
  font-size: 1.4em;
}
#fact {
  width: 40%;
}
@media print {
  .wrapper {
    visibility: hidden;
  }
}
</style>

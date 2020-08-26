<template>
  <canvas id="canvas" :width="this.width" :height="this.height"></canvas>
</template>

<script>
export default {
  data: function () {
    return {
      canvas: undefined,
      width: 1250,
      height: 375,
      title: "程式設計版",
      framems: 10,
      timer: undefined,
      backgroundColor: "#000000",
      titleColor: "white",
      wordColor: "white",
      shift: 0,
      words: [
        `console.log("Hello World!");`,
        `cout << "Hello World!\\n";`,
        `System.out.println("Hello World!");`,
        `printf("Hello World!");`,
        `Console.WriteLine("Hello World!");`,
        `System.Console.WriteLine("Hello World!")`,
        `echo "Hello World";`,
        `print_r("Hello World!");`,
        `var_dump("Hello World!");`,
        `不縮排是違法的行為！`,
        `兩格縮排還是四格縮排好呢？`,
        `Clean Code!`,
        `程式正常執行了，然而我卻不知道為何`,
        `先 Google 再發問!`,
        `遇到問題先搜索`,
        `沒有 bug？肯定有問題。`,
        `200 OK`,
        `403 Forbidden`,
        `404 Not Found`,
        `418 I'm a teapot.`,
        `狗才寫 JS，汪汪汪。`,
        `我大 JS 包山包海，真香`,
        `問完問題不要自刪`,
        `快逃RRR`,
        `我是應徵前端不是後端R`,
      ],
      sentences: [],
    };
  },
  created: function () {
    this.generateSentences();
    this.timer = setInterval(() => {
      this.draw();
    }, this.framems);
  },
  beforeDestroy: function () {
    clearInterval(this.timer);
  },
  methods: {
    shuffleWords() {
      // while (this.words.length < 40) this.words = this.words.concat(this.words);

      this.words = this.words
        .map((value) => ({ sort: Math.random(), content: value }))
        .sort((a, b) => a.sort - b.sort)
        .map((value) => value.content);
    },
    generateSentences() {
      this.shuffleWords();
      let cache = " ";

      for (let index in this.words) {
        cache += this.words[index] + " ";
        if (cache.length > 40) {
          this.sentences.push(cache);
          cache = " ";
        }
      }
      console.log(this.sentences);
    },
    async draw() {
      // console.log("Draw it!");
      let canvas = document.getElementById("canvas");
      if (!canvas.getContext) return false;
      canvas.style.backgroundColor = this.backgroundColor;

      let context = canvas.getContext("2d");
      context.clearRect(0, 0, canvas.width, canvas.height); // Clear content.

      await this.renderWords();
      await this.renderTitle();
    },
    renderTitle() {
      return new Promise((resolve) => {
        // console.log("Rendering title...");
        let context = document.getElementById("canvas").getContext("2d");
        let x = this.width * 0.225,
          y = this.height * 0.235,
          width = this.width * 0.55,
          height = this.height * 0.45;
        context.clearRect(x, y, width, height);

        context.font = `${this.height / 3}px Consolas`;
        context.textAlign = "center";
        context.textBaseline = "middle";
        context.fillStyle = this.titleColor;
        context.fillText(this.title, this.width / 2, this.height / 2);
        resolve();
      });
    },
    renderWords() {
      return new Promise((resolve) => {
        // console.log("Rendering words...");

        let context = document.getElementById("canvas").getContext("2d");

        let wordHeight = this.height / 10;
        context.font = `${wordHeight}px Consolas`;
        context.textBaseline = "alphabetic";
        context.fillStyle = this.wordColor;

        this.shift = this.shift + this.framems * 0.75;
        for (let index in this.sentences) {
          context.textAlign = index % 2 == 0 ? "right" : "left";
          let x = this.shift,
            y = (Number(index) + 1) * (wordHeight + 10) - 15;
          context.fillText(
            this.sentences[index],
            index % 2 == 0 ? x : this.width - x,
            y
          );
        }
        resolve();
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
canvas {
  border: 1px solid black;
}
</style>

<template>
  <div>
    <canvas id="index-canvas" @mousemove="moveHandler"></canvas>
    <span class="index-title">Hello World</span>
    <router-link class="index-href" to="/canvas/demo">canvas-demo</router-link>
    <div class="line"></div>
    <div class="box"></div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
      bubbleColor: ['#fe6e6e', '#f5fe6e', '#84fe6e', '#6ef7fe', '#7e6efe', '#fe6eec', '#fec46e'],
      prevTime: 0,
      location: [],
      timeLoop: [],
    };
  },
  mounted() {
    const cvs = document.getElementById('index-canvas');
    cvs.width = document.documentElement.clientWidth;
    cvs.height = document.documentElement.clientHeight;
  },
  methods: {
    moveHandler(e) {
      const now = +new Date();
      if (now - this.prevTime > 50) {
        const canvasContext = document.getElementById('index-canvas').getContext('2d');
        // 没处发一次事件，产生4个位置点，位置点最多有60个
        if (this.location.length >= 56) {
          this.location = this.location.slice(4);
        }
        for (let i = 0; i < 2; i += 1) {
          this.location.push({
            x: e.clientX,
            y: e.clientY,
            vx: 3 - (Math.random() * 6),
            vy: 3 - (Math.random() * 6),
            alpha: 0.8,
          });
        }

        // 终止定时器
        if (this.timeLoop) {
          clearInterval(this.timeLoop);
        }
        this.timeLoop = setInterval(() => {
          this.cvsRun(canvasContext);
        }, 30);

        // 设定反应时间
        this.prevTime = now;
      }
    },
    cvsRun(ctx) {
      const location = this.location;
      ctx.clearRect(0, 0, document.documentElement.clientWidth,
        document.documentElement.clientHeight);
      for (let i = 0; i < location.length - 1; i += 1) {
        this.location[i].x = this.location[i].x - this.location[i].vx;
        this.location[i].y = this.location[i].y - this.location[i].vy;
        this.location[i].alpha = this.location[i].alpha - 0.02;
        if (this.location[i].alpha <= 0) {
          this.location[i].alpha = 0;
        }
        const colorIndex = i % 7;
        ctx.fillStyle = this.bubbleColor[colorIndex];
        ctx.globalAlpha = this.location[i].alpha;
        ctx.beginPath();
        ctx.arc(this.location[i].x, this.location[i].y, 30, 0, 2 * Math.PI, true);
        ctx.closePath();
        ctx.fill();
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
a{
  text-decoration: none;
  color: #ebe6ff;
}
h1,h2,h3,h4,h5,h6{
  color: #fff;
}
#index-canvas{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}
.index-title{
  position: absolute;
  top: 100px;
  left: 50%;
  margin: 0 auto;
  color: white;
  font-size: 36px;
  animation: helloWorld 5s linear;
  text-shadow: 0px 0px 1px #fff;
  z-index: 101;
  opacity: 0;
}

@keyframes helloWorld {
  0% { transform: scale(1) rotateY(0deg) rotate(0deg); }
  25% { transform: scale(1.5) rotate(-30deg);}
  50% { transform: scale(1) rotateY(-45deg) rotate(-30deg); opacity: 1;}
  75% { transform: scale(1.5) rotate(-30deg);}
  100% { transform: scale(1) rotateY(0deg) rotate(0deg);opacity: 0;}
}

.index-href{
  position: absolute;
  top: 30%;
  left: 0;
  right: 0;
  text-align: center;
  font-size: 72px;
  color: #fa3310;
  text-shadow: 2px 2px 2px #763d2b;
  z-index: 99;
}
.line{
  position: absolute;
  top: 85%;
  width: 100%;
  background-color: #fff;
  height: 3px;
  animation: lineStart 3s linear;
  box-shadow: 2px 2px 10px #fff;
}

@keyframes lineStart {
  0% { left: 100%;}
  100% { left: 0;}
}

.box{
  position: absolute;
  top: -5%;
  left: 20%;
  width: 20px;
  height: 20px;
  background-color: #fff;
  border-radius: 50%;
  animation: boxStart 3s ease-in infinite;
}
@keyframes boxStart {
  0% { top: -5%; left: 20%;}
  40% { top: 88%; left: 25%;}
}
</style>

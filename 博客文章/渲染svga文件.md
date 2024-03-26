#                                              **使用svga文件渲染**

# **1.代码写法**

```
<template>

  <div id="demoCanvas" class="demoCanvas"></div>
</template>

<script>
import SVGA from 'svgaplayerweb';
// import SVG from '../public/svga/date.svga'
export default {
  methods: {
    initMachineSVGA() {
     
      const player = new SVGA.Player('#demoCanvas');
      const parser = new SVGA.Parser('#demoCanvas');
      parser.load('/svga/date.svga', (videoItem) => {
        player.setVideoItem(videoItem);
        player.startAnimation();
        console.log(videoItem, '成功');
      }, (error) => {
        console.log(error, '失败');
      });
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.initMachineSVGA();
    });
  },
};
</script>

<style>
/* Add your styles here */
</style>
```

# 2.其他属性

## （1）SVGA.Player 用于控制动画的播放和停止

```
const guideFn = () => {
  // 获取id的dom元素
  var player = new SVGA.Player(`#guide${timeId.value}`);
  var parser = new SVGA.Parser();
 
  parser.load("/svgas/dj.svga", (videoItem) => {
    // 你的svga文件路径
    player.setVideoItem(videoItem);
    player.startAnimation(); // 开始播放动画
    player.loops = 1; // 设置循环次数
    player.setWrapMode(1); // 设置填充模式
    player.pauseAnimation(); // 暂停动画
    player.stopAnimation(); // 停止动画
    player.stepToFrame(10, true); // 跳转到第10帧并播放
  });
};
```

## （2）Properties

int loops; - 动画循环次数，默认值为 0，表示无限循环。

BOOL clearsAfterStop; - 默认值为 true，表示当动画结束时，清空画布。

string fillMode; - 默认值为 Forward，可选值 Forward / Backward，当 clearsAfterStop 为 false 时，Forward 表示动画会在结束后停留在最后一帧，Backward 则会在动画结束后停留在第一帧。

Svgaplayer提供了一系列控制方法来管理SVGA动画的播放，包括：



## （3）Methods

constructor (canvas); - 传入 #id 或者 CanvasHTMLElement 至第一个参数

startAnimation(reverse: boolean = false); - 从第 0 帧开始播放动画

startAnimationWithRange(range: {location: number, length: number}, reverse: boolean = false); - 播放 [location, location+length] 指定区间帧动画

pauseAnimation(); - 暂停在当前帧

stopAnimation(); - 停止播放动画，如果 clearsAfterStop === true，将会清空画布

setContentMode(mode: "Fill" | "AspectFill" | "AspectFit"); - 设置动画的拉伸模式

setClipsToBounds(clipsToBounds: boolean); - 如果超出盒子边界，将会进行裁剪

clear(); - 强制清空画布

stepToFrame(frame: int, andPlay: Boolean); - 跳到指定帧，如果 andPlay === true，则在指定帧开始播放动画

stepToPercentage(percentage: float, andPlay: Boolean); - 跳到指定百分比，如果 andPlay === true，则在指定百分比开始播放动画

setImage(image: string, forKey: string, transform: [a, b, c, d, tx, ty]); - 设定动态图像, transform 是可选的, transform 用于变换替换图片

setText(text: string | {text: string, family: string, size: string, color: string, offset: {x: float, y: float}}, forKey: string); - 设定动态文本

clearDynamicObjects(); - 清空所有动态图像和文本

## （4）Callback Method

onFinished(callback: () => void): void; - 动画停止播放时回调

onFrame(callback: (frame: number): void): void; - 动画播放至某帧后回调

onPercentage(callback: (percentage: number): void): void; - 动画播放至某进度后回调

（5）SVGA.Parser

SVGA.Parser 用于加载远端或 Base64 动画，并转换成 VideoItem。

跨域的 SVGA 资源需要使用 CORS 协议才能加载成功。

如果你需要加载 Base64 资源，或者 File 资源，这样传递就可以了 load(File) 或 load('data:svga/2.0;base64,xxxxxx')。
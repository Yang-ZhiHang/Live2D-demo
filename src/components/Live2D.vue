<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import * as PIXI from "pixi.js";
import { Live2DModel } from "pixi-live2d-display/cubism4";

window.PIXI = PIXI; // 用于 pixi-live2d-display 内部调用

let pixiCanvas; // 放置 live2d 模型的画布
let live2DModel;

const live2DCanvas = ref();

onMounted(async () => {
  pixiCanvas = new PIXI.Application({
    view: live2DCanvas.value,
    autoStart: true,
    width: 600,
    height: 1250,
    backgroundAlpha: 0,
  });

  // live2d 资源路径
  live2DModel = await Live2DModel.from(
    "/hiyori_free_zh/hiyori_free_t08.model3.json"
  );

  pixiCanvas.stage.addChild(live2DModel); // 将模型添加到画布上
  live2DModel.scale.set(0.2); // 设置合适的缩放比例

  // 点击交互效果
  // live2DModel.on("pointerdown", async () => {
  //   live2DModel.expression(expression)
  //   live2DModel.motion(motion)
  //   /* 函数内部逻辑 */
  // });
  // live2DModel.buttonMode = true;
  // live2DModel.interactive = true;
});

onUnmounted(() => {
  // 清理事件监听
  window.removeEventListener("keydown", () => {});
  pixiCanvas?.destroy();
  live2DModel?.destroy();
});
</script>

<template>
  <canvas ref="live2DCanvas"></canvas>
</template>

<style scoped></style>

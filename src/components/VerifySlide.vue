<template>
  <div class="verify_slide">
    <div class="drop_left" :style="{ width: moveX }"></div>
    <div
      class="move"
      :class="{ active: isDrop }"
      @touchstart="start"
      @mousedown="start"
    >
      拖
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";

@Component({})
export default class VerifySlide extends Vue {
  mounted() {
    console.log("init");
    this.init();
  }
  private isDrop: boolean = false;
  private moveX: string = "0px";
  private startX: number = 0; // 初始位置
  private init() {
    window.removeEventListener("touchmove", e => {
      this.move(e);
    });
    window.removeEventListener("mousemove", e => {
      this.move(e);
    });

    //鼠标松开
    window.removeEventListener("touchend", () => {
      this.end();
    });
    window.removeEventListener("mouseup", () => {
      this.end();
    });

    window.addEventListener("touchmove", e => {
      this.move(e);
    });
    window.addEventListener("mousemove", e => {
      this.move(e);
    });

    //鼠标松开
    window.addEventListener("touchend", () => {
      this.end();
    });
    window.addEventListener("mouseup", () => {
      this.end();
    });
  }
  private move(e: MouseEvent | TouchEvent) {
    if (this.isDrop) {
      let x: number = 0;
      if (e instanceof TouchEvent) {
        // 移动端
        x = e.touches[0].pageX;
      } else if (e instanceof MouseEvent) {
        x = e.clientX;
      }
      console.log("初始", this.startX);
      if (x - this.startX <= 0) {
        this.moveX = "0px";
      } else if (x - this.startX >= 260) {
        this.moveX = 260 + "px";
      } else {
        this.moveX = x - this.startX + "px";
      }
      console.log("移动", x);
    }
  }
  private end() {
    console.log("松开");
    this.isDrop = false;
    this.moveX = "0px";
  }
  private start(e: MouseEvent | TouchEvent) {
    let x: number = 0;
    if (e instanceof TouchEvent) {
      // 移动端
      x = e.touches[0].pageX;
    } else if (e instanceof MouseEvent) {
      x = e.clientX;
    }
    this.startX = x;
    this.isDrop = true;
    e.stopPropagation();
  }
}
</script>

<style lang="scss" scoped>
.verify_slide {
  border-radius: 3px;
  border: 1px solid rgb(202, 202, 202);
  width: 300px;
  height: 40px;
  display: flex;
  .move {
    position: relative;
    left: 0px;
    transition: all 0.02s;
    user-select: none;
    cursor: pointer;
    text-align: center;
    margin-left: -1px;
    line-height: 40px;
    width: 40px;
    height: 40px;
    border-left: 1px solid rgb(202, 202, 202);
    border-right: 1px solid rgb(202, 202, 202);
  }
  .drop_left {
    background: green;
    width: 20px;
    height: 100%;
  }
  .active {
    background: rgb(197, 197, 197);
  }
}
</style>
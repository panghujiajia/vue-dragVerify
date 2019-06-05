<template>
  <div>
    <div class="dragOut" :style="{width:dragWidth+'px',height:dragHeight+'px'}">
      <p :style="{lineHeight:dragHeight +'px'}">{{dragStatus ? successText : defaultText}}</p>
      <div class="successBox" :style="{width:dragLeft +'px'}"></div>
      <div
        class="dragBall"
        :style="{left:dragLeft +'px',width:ballWidth+'px',lineHeight:dragHeight +'px'}"
        @mousedown="handleMousedown"
      >>></div>
    </div>
  </div>
</template>
<script>
export default {
  name: "dragVerify",
  props: {
    dragWidth: {
      //容器的宽度
      type: Number,
      default: 350
    },
    dragHeight: {
      //容器的高度
      type: Number,
      default: 40
    },
    ballWidth: {
      //滑块的宽度
      type: Number,
      default: 40
    },
    defaultText: {
      // 滑块滑动前的提示语
      type: String,
      default: "滑动到右侧验证"
    },
    successText: {
      // 滑块滑动成功后的提示语
      type: String,
      default: "验证通过"
    }
  },
  data() {
    return {
      lock: false, //检测鼠标按下后才监听鼠标移动事件
      dragStatus: false, //滑块状态
      dragLeft: 0 //滑块的初始坐标
    };
  },
  mounted() {
    // 给window绑定鼠标松开和移动事件
    window.addEventListener("mouseup", this.handleMouseup);
    window.addEventListener("mousemove", this.handleMousemove);
  },
  methods: {
    handleMousedown(e) {
      //检测用户是否完成验证
      if (!this.dragStatus) {
        this.lock = true;
        this.startX = e.screenX; //鼠标按下的X坐标
      }
    },
    handleMousemove(e) {
      var startX = this.startX; //鼠标按下的X坐标
      var moveX = e.screenX; //鼠标移动的X坐标
      var distance = moveX - startX; //滑块移动的距离
      if (this.lock) {
        if (distance >= this.dragWidth - this.ballWidth) {
          distance = this.dragWidth - this.ballWidth;
        } else if (distance <= 0) {
          distance = 0;
        }
        this.dragLeft = distance;
      }
    },
    handleMouseup() {
      // 松开判定是否拉到头了，没有就还原
      if (this.dragLeft != this.dragWidth - this.ballWidth) {
        this.dragStatus = false;
        this.dragLeft = 0;
      } else {
        this.dragStatus = true; //滑块状态修改
      }
      this.lock = false;
    }
  }
};
</script>
<style lang="less" scoped>
.dragOut {
  position: relative;
  overflow: hidden;
  height: 100%;
  width: 100%;
  box-shadow: 0 0 2px #dedede;
  -webkit-user-select: none;
  background: #ccc;
  user-select: none;
  p {
    position: absolute;
    z-index: 20;
    width: 100%;
    height: 100%;
    color: #fff;
    text-shadow: 0 0 5px #222;
  }
  .successBox {
    height: 100%;
    width: 0;
    position: absolute;
    background: #67c23a;
  }
  .dragBall {
    position: absolute;
    left: 0;
    z-index: 30;
    height: 100%;
    background: #fff;
    cursor: move;
  }
}
</style>


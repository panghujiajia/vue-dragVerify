<template>
  <div>
    <div class="dragOut" :style="{width:dragWidth+'px',height:dragHeight+'px'}">
      <p v-if="dragStatus" :style="{lineHeight:dragHeight +'px',color:'#fff'}">{{successText}}</p>
      <p v-else :style="{lineHeight:dragHeight +'px',color:'#777'}">{{defaultText}}</p>
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
    status: {
      // 滑块状态
      type: Boolean,
      default: false
    },
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
      dragStatus: this.status, //滑块默认状态，由初始传入的值决定
      dragLeft: 0 //滑块的初始坐标
    };
  },
  watch: {
    // 传入的值变化后，修改相应的dragStatus，并且将滑块复原
    status(newValue, oldValue) {
      this.dragStatus = newValue;
      if (!newValue) {
        this.dragLeft = 0;
      }
    }
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
      var timer;
      // 节流
      if (!timer) {
        timer = setTimeout(() => {
          if (this.lock) {
            if (distance >= this.dragWidth - this.ballWidth) {
              distance = this.dragWidth - this.ballWidth;
            } else if (distance <= 0) {
              distance = 0;
            }
            this.dragLeft = distance;
          }
          timer = null;
        }, 20);
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
      this.$emit("getDragStatus", this.dragStatus);
      this.lock = false;
    }
  }
};
</script>
<style lang="less" scoped>
.dragOut {
  position: relative;
  height: 100%;
  width: 100%;
  box-shadow: 0 0 2px #dedede;
  border-radius: 5px;
  -webkit-user-select: none;
  background: #ebebeb;
  user-select: none;
}
p {
  position: absolute;
  z-index: 20;
  width: 100%;
  height: 100%;
}
.successBox {
  height: 100%;
  width: 0;
  position: absolute;
  background: #a5dd8a;
  border-radius: 5px 0 0 5px;
}
.dragBall {
  position: absolute;
  left: 0;
  z-index: 30;
  height: 100%;
  background: #fff;
  border: 1px solid #cecece;
  cursor: move;
}
</style>


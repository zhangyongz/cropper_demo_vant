<template>
  <div class="cropper_box" v-show="show">
    <div class="wrapper">
      <vue-cropper ref="cropper" :img="img" :output-size="option.outputSize" :output-type="option.outputType"
                   :info="false" :full="option.full" :can-move="canMove" :can-move-box="canMoveBox" :fixed-box="fixedBox"
                   :original="option.original" :auto-crop="option.autoCrop" :auto-crop-width="autoCropWidth"
                   :auto-crop-height="autoCropHeight" :center-box="option.centerBox" :high="option.high" :fixedNumber="fixedNumber"
                   :fixed="fixed" :enlarge="1"></vue-cropper>
    </div>
    <div class="btn_wrapper">
      <span class="btn_item" @click="cancel">取消</span>
      <span class="btn_item" @click="sure">完成</span>
    </div>
  </div>
</template>

<script>
import { VueCropper } from 'vue-cropper'
export default {
  name: 'cropper',
  data () {
    return {
      option: {
        outputSize: 1,
        full: false,
        outputType: 'jpeg',
        original: false,
        autoCrop: true,
        centerBox: true,
        high: true
      }
    }
  },
  model: {
    prop: 'show',
    event: 'change'
  },
  props: {
    show: {
      type: Boolean,
      default: false
    },
    img: String, // 裁剪图片的地址 url 地址 || base64 || blob
    autoCropWidth: { // 默认生成截图框宽度 0~max
      type: Number,
      default: 200
    },
    autoCropHeight: { // 默认生成截图框高度 0~max
      type: Number,
      default: 100
    },
    fixed: { // 是否开启截图框宽高固定比例 true | false
      type: Boolean,
      default: false
    },
    fixedNumber: Array, // 截图框的宽高比例 [宽度, 高度]
    fixedBox: { // 固定截图框大小 不允许改变 true | false
      type: Boolean,
      default: false
    },
    canMove: { // 上传图片是否可以移动 true | false
      type: Boolean,
      default: true
    },
    canMoveBox: { // 截图框能否拖动 true | false
      type: Boolean,
      default: true
    }
  },
  watch: {
    show () {
      if (this.show) {
        this.$refs.cropper.refresh()
      }
    }
  },
  components: {
    VueCropper
  },
  methods: {
    sure () {
      this.$emit('change', false)
      this.$refs.cropper.getCropData((data) => {
        this.$emit('backImg', data)
      })
    },
    cancel () {
      this.$emit('change', false)
      this.$emit('cancel')
    }
  }
}
</script>

<style lang="less">
.cropper_box {
  width: 100%;
  position: fixed;
  left: 0;
  top: 0;
  z-index: 1000;
  .wrapper {
    width: 100%;
    height: 90vh;
    .vue-cropper {
      background: #000000;
    }
  }
  .btn_wrapper {
    height: 10vh;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #f7f7f7;
    background-color: #000000;
    position: relative;
    &:before {
      content: " ";
      position: absolute;
      left: 0;
      top: 0;
      right: 0;
      height: 1px;
      border-top: 1px solid #3c3c3c;
      color: #3c3c3c;
      transform-origin: 0 0;
      transform: scaleY(0.5);
    }
    .btn_item {
      height: 100%;
      line-height: normal;
      padding: 0 20px;
      font-size: 14px;
      display: flex;
      align-items: center;
    }
  }
}
</style>

<template>
  <div class="home">
    <van-uploader :after-read="afterRead" v-model="fileList" />
    <v-cropper v-show="show" :img="img" @backImg="backImg" @cancel="cancel"></v-cropper>
  </div>
</template>

<script>
import { Uploader } from 'vant'
import cropper from '@/components/cropper'
import vCropper from '@/components/cropper/cropper'
export default {
  name: 'Home',
  data () {
    return {
      file: {},
      fileList: [],
      show: false,
      img: ''
    }
  },
  components: {
    'van-uploader': Uploader,
    'v-cropper': vCropper
  },
  methods: {
    cancel () {
      const file = this.file
      this.show = false
      file.content = this.file.content
      file.status = 'uploading'
      file.message = '上传中...'
      this.uploadImg({
        result: this.file.content
      }).then((res) => {
        file.status = 'success'
        file.message = '上传成功'
      }).catch(() => {
        file.status = 'failed'
        file.message = '上传失败'
      })
    },
    backImg (result) {
      const file = this.file
      this.show = false
      file.content = result
      file.status = 'uploading'
      file.message = '上传中...'
      this.uploadImg({
        result
      }).then((res) => {
        file.status = 'success'
        file.message = '上传成功'
      }).catch(() => {
        file.status = 'failed'
        file.message = '上传失败'
      })
    },
    uploadImg (data) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(data)
        }, 1000)
      })
    },
    afterRead1 (file) {
      this.show = true
      this.img = file.content
      this.file = file
    },
    afterRead (file) {
      const $this = this
      cropper.show({
        backImg (result) {
          file.content = result.content
          file.status = 'uploading'
          file.message = '上传中...'
          $this.uploadImg({
            name: result.fileName,
            content: result.content.substring(result.content.indexOf(',') + 1)
          }).then((res) => {
            console.log(res)
            file.status = 'success'
            file.message = '上传成功'
          }).catch(() => {
            file.status = 'failed'
            file.message = '上传失败'
          })
        },
        file
      })
    }
  }
}
</script>

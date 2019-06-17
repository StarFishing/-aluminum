<template>
  <div>
    <button @click="choce">选择文件</button>
    <input ref="upload"
           type="file"
           multiple
           placeholder="选择文件"
           style="position: absolute; width: 1px; height: 1px; opacity: 0; z-index: -1;"
           @change="selectFile">
    <div style="width:300px;height:300px;background:grey"
         @dragenter="handledragenter"
         @dragover="handledragover"
         @drop="handledrop">
      拖拽上传
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      filelist: {}
    }
  },
  methods: {
    selectFile() {
      this.aa()
    },
    handledragenter(e) {
      e.stopPropagation()
      e.preventDefault()
    },
    handledragover(e) {
      e.stopPropagation()
      e.preventDefault()
    },
    handledrop(e) {
      e.stopPropagation()
      e.preventDefault()
      var dt = e.dataTransfer
      var files = dt.files
      console.log(files)
    },
    choce() {
      this.$refs.upload.click()
    },
    aa() {
      // var xhr = new XMLHttpRequest()
      var formData = new FormData()
      var fileInput = this.$refs.upload
      var file = fileInput.files
      // console.log(file)
      formData.append('myFile', file[0])
      // console.log(formData)
      // console.log(file[0].type)
      this.filelist.name = file[0].name
      this.filelist.size = file[0].size
      // console.log(this.filelist)
      // xhr.open('POST', '/uploaddd')

      // xhr.onload = function() {
      //   if (this.status === 200) {
      //     // 对请求成功的处理
      //   }
      // }

      // xhr.send(formData)
      // xhr = null
    }
  }
}
</script>
